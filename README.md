# Modern Data Pipeline Framework

This repository offers a comprehensive, containerized framework for building and orchestrating modern data pipelines using **Apache Airflow**, **dbt**, and **Snowflake**.

## Overview

- **Apache Airflow**: Schedule, manage, and monitor complex workflows through intuitive Directed Acyclic Graphs (DAGs). Pre-configured DAGs in this repo help you get started quickly.
- **dbt (data build tool)**: Transform your data using modular SQL-based transformations, tests, and documentation. The integrated dbt project provides a solid foundation for developing and maintaining ELT processes.
- **Snowflake**: Utilize Snowflake as your cloud data warehouse, leveraging its scalability and performance for storing and querying data.

## Key Features

- **Containerized Development**: Built on Docker, ensuring a consistent development environment that simplifies both local testing and deployment.
- **Pre-built Project Structure**: Comes with a ready-to-use layout including Airflow DAGs, a structured dbt project, and configuration files (`airflow_settings.yaml`, `profiles.yml`) for seamless integration.
- **Flexible Configuration**: Easily manage your credentials, connections, and environment variables—whether through local `.env` files, Airflow settings, or dbt profiles.
- **Seamless Integration**: Orchestrate data ingestion into Snowflake and trigger dbt transformations automatically, ensuring your analytics tables stay up-to-date.
- **Production Deployment Ready**: With support for deployment via Astronomer, you can transition from local development to a robust, production-grade Airflow environment with minimal effort.

## Getting Started

1. **Local Setup**:  
   - Install dependencies using `pip install -r requirements.txt`.
   - Launch your Airflow environment with `astro dev start`, which spins up containers for the Postgres metadata database, Airflow webserver, scheduler, and triggerer.
   - Access the Airflow UI at [http://localhost:8080](http://localhost:8080) (default credentials: admin/admin).

2. **dbt Integration**:  
   - Navigate to the `dbt` directory to manage your transformations.
   - Use standard dbt commands such as `dbt debug`, `dbt run`, and `dbt test` to validate, compile, and execute your models.
   - Ensure your `profiles.yml` is configured with your Snowflake credentials.

3. **Snowflake Connectivity**:  
   - Set up Airflow connections to Snowflake either via the Airflow UI or by configuring `airflow_settings.yaml`.
   - Update your dbt `profiles.yml` to establish secure, environment-specific Snowflake connections.

4. **Deployment**:  
   - When ready, deploy your solution to the cloud using Astronomer. Authenticate with `astro login`, then build, push, and deploy your image to a production-grade Airflow environment.

This framework streamlines the process of extracting, transforming, and loading data—empowering your data operations with automation, scalability, and ease of management.
