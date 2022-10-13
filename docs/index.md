# Upsolver Self-Guided SQLake Hands-On Labs

## Welcome to Upsolver SQLake

SQLake is ...

The initial lab will take around 20 minutes, and includes a general walkthrough, a brief tour of the user interface, and creation of your first pipeline.  From there, you can continue on to additional labs will walk you through the power of Upsolver's streaming ETL capabilities along with it's [Kappa architecture](https://www.oreilly.com/radar/questioning-the-lambda-architecture/).

The Kappa architecture has several benefits:

* Handle all batch and streaming use cases with a single architecture (say "no" to Lambda architecture)
* One codebase that is always in sync
* One set of infrastructure and technology
* The infrastructure is near real-time, scalable, reliable (idempotent), and cost effective
* Improved data quality with guaranteed ordering and no mismatches (exactly-once processing guarantee)
* No need to re-architect for new use cases

At the end of each lab, we will propose some additional self-guided exercises that you may complete if you wish to expand your learning of Upsolver.

---

## Let's Get Started - [Sign-up](https://sqlake.upsolver.com/signup)
Navigate to the sign-up page, and click the 'Sign Up' link at the bottom of the Log in window.
![Signup Window](/sqlake-workshop/img/Signup.png "SQLake Signup Dialog")

---

## Exercise 1
Now lets build our first pipeline.  In this pipeline, we are going to be taking some streaming data from an S3 bucket that Upsolver provides.  We will ingest that raw data into a staging table, where we store an immutable copy of it in its raw form, catalog its structure and metadata, and make it accessible for querying and downstream processing.  We will then create a series of transformations on this raw data, and query that transformed data with Athena.
![Excercise diagram](/sqlake-workshop/img/img1a.png "Diagram of exercise 1")

Upon first login, you are presented with an introduction to the platform.  Feel free to step through the wizard until you get to the “Done” button.  
![SQLake Greeting](/sqlake-workshop/img/img1b.png)

When you finish the initial wizard you are presented with a set of out of the box templates.  These templates can serve as introductions to common pipeline use cases that you may wish to explore.  The “Templates with Sample Data” section, include everything that you need to build a full end to end pipeline, and is where we will start.  The other templates provided will require you to connect to your own data sources, i.e. Kinesis or Kafka, streams, but include example scripts and commands to work with whatever data that you have available.
In this lab, we are going to start with the S3 to Athena (sample data) template.
![SQLake Templates](/sqlake-workshop/img/img1c.png)

### UI walkthrough
Before we get into the template itself, lets walk through the core components of the UI.  
![SQLake UI Explanation](/sqlake-workshop/img/img1d.png)

There is a back button in the upper left hand corner of the screen that you can use to go back to your worksheets list, choose a different template to work with, or navigate to other sections of the SQLake UI.

The center of the title bar shows you the worksheet that you are currently working with.  This current worksheet was based on the template that we chose.  The down arrow next to the worksheet name provides a menu that allows you to rename your worksheet, duplicate it (often used to create backup copies), download your worksheet as a SQL file, delete your worksheet, or make it public.  Worksheets are private by default, only visible to you, but if you have multiple users in the same organization, you can share worksheets with others by making them public.

Continuing to the right, there is a SQL Snippet button to include common code blocks into your worksheet, and finally a “Preview Job” and “Run” button that we will see in action later in the workshop.

| Catalog - Displays available connections |  |
:--- | ---:
| * By default, every user has two connections<br>Types of connections<br> * GLUE_CATALOG<br> * S3<br> * KINESIS<br> * KAFKA<br> * SNOWFLAKE<br> | ![SQLake UI Catalog View](/sqlake-workshop/img/img1e.png) |

| Help and documentation |  |
:--- | :---
| ![SQLake Learn & Explore](/sqlake-workshop/img/img1f.png) | * Searchable documentation<br> * Ask general questions on Slack<br> * Contact support if something doesn’t work as expected |

>Our Sample Data<br>
>The sample data that we will use throughout this exercise is streaming orders data from a fictional e-commerce website.  We are working as data engineers and being asked to transform this data for a variety of purposes while populating a data lake for historical reporting

**Take a look at your Worksheet (Template)**

Step 1: Create connection to S3

```sql
/*
   1. Create an S3 connection to manage IAM credentials for jobs that need to access data in S3
*/
CREATE S3 CONNECTION upsolver_s3_samples
   AWS_ROLE = 'arn:aws:iam::949275490180:role/upsolver_samples_role'
   EXTERNAL_ID = 'SAMPLES'
   READ_ONLY = TRUE;
```

> To run a command:
> Place your cursor within the command and click the Run button in the upper right
Click the + button to the left of the line number, and select “Run Statement”
> Press a keyboard shortcut (i.e. *<command + enter>* on Mac or *<control + enter>* on Windows

**Browse Newly Created Connection**

With your connection created, you will see it appear in the catalog view on the left.  Browse through the connection, noticing the bucket/folder structure of the sample data.

![Catalog Update](/sqlake-workshop/img/img1g.png)

Step 2: Create a staging table for data ingestion

With our source connection created, scroll down to Step 2 in the worksheet, where we will create our staging table to load our raw data into.  Creating this table is as simple as giving your table a name, and choosing how you want your data to be partitioned.  We do not have to define the full schema of the table, as it will be built out as streaming data is ingested.  

In most use cases, staging tables will be partitioned by the $event_date system column, which is the timestamp of when the incoming event was processed.

```sql
/*
   2. Create an empty table to use as staging for the raw data.
*/
CREATE TABLE default_glue_catalog.database_714130.orders_raw_data()
   PARTITIONED BY $event_date;
```

> Go ahead and run this command, and view your newly created staging table in the catalog view.

**View Your Table in the Catalog**

Notice that since your table is empty, there are no columns, other than two system columns.

![Catalog orders_raw_data Table](/sqlake-workshop/img/img1h.png)
