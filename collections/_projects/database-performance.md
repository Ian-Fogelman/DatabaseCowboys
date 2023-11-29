---
title: "Database Performance"
description: "Experience a significant boost in efficiency and responsiveness with our database performance improvements."
date: 2018-12-20
weight: 3
header_transparent: false
thumbnail: "/assets/images/database-performance-5.jpg"
image: "/assets/images/database-performance-5.jpg"
client: "Brisbane City Council"

hero:
  enabled: true
  heading: "Database Performance"
  sub_heading: "Experience a significant boost in efficiency and responsiveness with our database performance improvements."
  text_color: "#FFFFFF"
  background_color: ""
  background_gradient: true
  background_image: "/assets/images/database-performance-5.jpg"
  background_image_blend_mode: "overlay" # "overlay", "multiply", "screen"
  fullscreen_mobile: false
  fullscreen_desktop: false
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

A client was having performance issues with a web application because of the back end SQL Server database. This project involved identifying the performance bottleneck, scripting a reproducible test case to run in a test environment, and elevating the solution to production.

# Solutions Used

- SQL Server Profiler
- Python
- SQL Refactoring
- Dbt

# Project Details

The performance issues were systemic application lag that would result in the checkout process for the retailer's point of sales systems hanging and causing unpleasant user experiences. To identify the issue, SQL Server Profiler was used to monitor the database traffic to the production database. You can think of SQL Server Profiler as a radar to your SQL server which shows all database traffic and exactly how long and against what resources transactions are run on your database.

Once there was enough sample data collected with the profiler tool the logs were reviewed. We were able to identify multiple issues which could be causality for the application slowness. Next we generated python scripts which would replicate the database traffic patterns observed in the tracer profiles. This would allow us to test possible solutions in a different environment with the same conditions in production. 

Now that we could reproduce the database load with the Python scripts, we dug into various solutions which could help alleviate the slow SQL response to the application. For this particular issue two things stood out, one was the over indexing of multiple tables which negatively impacted the writes to the database. And the fact that some of the tables used in the production database were being accessed via a linked server.

For the SQL refactor phase of the project, we analyzed index usage on the tables with the slowest response times. We then removed the indexes with the lowest utilization, which were causing the lag in writes. We also advised that the tables which were being access via linked server be brought into the production SQL server instance so that the additional latency could be resolved. 

Lastly for the deployment piece of the project, we utilized a framework called Database Build Tools (DBT). DBT offers connectors for the most popular RDMS’s and at its core helps with deploying your database changes to multiple environments and source controlling those changes. With multiple DBT profiles, we can deploy to the test and production environments the same changes within minutes. We simply pointed DBT to the production profile and deployed the same changes we had successfully tested in the test environment to production.

# Impact and Results

The impact from the changes we made resulted in a 50% decrease in checkout time for the retailer's POS system. Previous database behaviors which could take from 30-45 seconds per transaction were lowered to 3-4 seconds (90%). The client is also now primed to use DBT as their deployment mechanism and has been trained on the advantages of the tool to better manage their data architecture and application needs in the future.

