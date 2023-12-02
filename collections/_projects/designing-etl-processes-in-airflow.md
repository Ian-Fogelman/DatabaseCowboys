---
title: "ETL Processes In Airflow"
description: "Redesigning manual processes to be scheduled Airflow jobs that use custom Python code."
date: 2018-12-20
weight: 1
header_transparent: false
thumbnail: "/assets/images/etl-processes.webp"
image: "/assets/images/etl-processes.webp"
client: "Zero Developments Pty Ltd"

hero:
  enabled: true
  heading: "ETL Processes In Airflow"
  sub_heading: "Redesigning manual processes to be scheduled Airflow jobs that use custom Python code."
  text_color: "#FFFFFF"
  background_color: ""
  background_gradient: true
  background_image: "/assets/images/etl-processes.webp"
  background_image_blend_mode: "overlay" # "overlay", "multiply", "screen", "false"
  fullscreen_mobile: false
  fullscreen_desktop: false
  height: "600px"
  buttons:
    enabled: false
    list:
      - text: "Buy Now"
        url: "https://www.zerostatic.io/theme/jekyll-advance/"
        external: true
        fa_icon: false
        size: large
        outline: true
        style: "primary"
---

# Business Problem

This project was an orchestration of several data centric tasks that were currently being done manually. Automating these tasks would free up staff hours and reduce data keying errors that are common when tasks are performed manually. To help facilitate the task by task nature of this project, Apache Airflow was chosen. Airflow is a dynamic workflow management framework that allows you to design directed acyclic graphs (DAGs) and provide a visual representation layer of the entire process. Airflow is Python native, anything you can do in Python you can orachest in Airflow which gives its many advantages.

# Solutions Used

- Apache Airflow
- Python
- Python Modules
  - Requests
  - SQLAlchemy
  - Pandas
  - Paramiko
  - OS
  - JSON

# Project Details

The client for this request was a small technology firm of about 10 people. The client had an hour entry system in which the employees were required to allocate weekly hours to project work that was used to bill clients. Every month the finance directory was required to pull the data from the timesheet entry software to a billing application that would then bill the clients for that month's work. 

As you can imagine this was a time consuming monthly task, which required manual intervention and multiple levels of cross validation to ensure the billing hours that got to the client were accurate. Luckily using Airflow and Python this complicity can be broken down into several “tasks”, tasks are logical units of work that make up your DAG, you can think of the DAG as an entire process. Tasks can utilize python functions and special Airflow runtime configurations to help design the DAG exactly how you need it. The other great thing about Airflow is everything runs on a containerized environment. 


With Python especially keeping your Python and module versions aligned can be tricky. Python modules are continuously releasing updated versions which can break your application code if run with a different version than the one originally written for. The native Docker deployment mechanism of Airflow allows you to specify the versions of all the python modules used in your DAG in a requirements.txt file so this pain point is eliminated. This helps ensure that your DAG can run in both Production and Testing environments the exact same way.

For this project we were able  to:  

- Use the Python Requests library to retrieve the hours from the hour capture software.
- Store the data in memory with Pandas and manipulate / clean data as needed.
- Persist the Pandas data for an audit layer to a data warehouse using SQLAlchemy.
- Construct an invoice document from the in-memory Pandas data and write it to an SFTP server.
- Allow for a validation and final approval phase. This allowed the Finance director an opportunity to finalize the sending of the invoices to each client.

# Impact and Results

All of this was done using Airflow, Airflow and Python are a great combination of technology that can allow you to automate nearly any task. 
Impact and Results

As a result of this project, the company saved approximately 10% of weekly staff hours and observed an increase in billing accuracy of 200%. Less invoices are disputed, less time is taken on invoice generation, and overall satisfaction for both the clients and employees of the company increased as a result of automating these tasks.
