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

## Let's Get Started - [Sign-up](https://sqlake.upsolver.com/signup)
Navigate to the sign-up page, and click the 'Sign Up' link at the bottom of the Log in window.
![Signup Window](/img/Signup.png "SQLake Signup Dialog")

## Exercise 1
Now lets build our first pipeline.  In this pipeline, we are going to be taking some streaming data from an S3 bucket that Upsolver provides.  We will ingest that raw data into a staging table, where we store an immutable copy of it in its raw form, catalog its structure and metadata, and make it accessible for querying and downstream processing.  We will then create a series of transformations on this raw data, and query that transformed data with Athena.
![Excercise diagram](/img/Exercise1/img1a.png "Diagram of exercise 1")

Upon first login, you are presented with an introduction to the platform.  Feel free to step through the wizard until you get to the “Done” button.  
![SQLake Greeting](/img/Exercise1/img1b.png)

When you finish the initial wizard you are presented with a set of out of the box templates.  These templates can serve as introductions to common pipeline use cases that you may wish to explore.  The “Templates with Sample Data” section, include everything that you need to build a full end to end pipeline, and is where we will start.  The other templates provided will require you to connect to your own data sources, i.e. Kinesis or Kafka, streams, but include example scripts and commands to work with whatever data that you have available.
In this lab, we are going to start with the S3 to Athena (sample data) template.
![SQLake Templates](/img/Exercise1/img1c.png)

### UI walkthrough
Before we get into the template itself, lets walk through the core components of the UI.  
![SQLake UI Explanation](/img/Exercise1/img1d.png)

There is a back button in the upper left hand corner of the screen that you can use to go back to your worksheets list, choose a different template to work with, or navigate to other sections of the SQLake UI.

The center of the title bar shows you the worksheet that you are currently working with.  This current worksheet was based on the template that we chose.  The down arrow next to the worksheet name provides a menu that allows you to rename your worksheet, duplicate it (often used to create backup copies), download your worksheet as a SQL file, delete your worksheet, or make it public.  Worksheets are private by default, only visible to you, but if you have multiple users in the same organization, you can share worksheets with others by making them public.

Continuing to the right, there is a SQL Snippet button to include common code blocks into your worksheet, and finally a “Preview Job” and “Run” button that we will see in action later in the workshop.

---

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

## Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.
