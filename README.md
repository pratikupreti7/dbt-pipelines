This repository brings together Apache Airflow, dbt, and Snowflake to create a modern data pipeline framework. Use this setup to orchestrate data ingestion, transformation, and loading into Snowflake—all from one cohesive environment.

Table of Contents
Overview
Project Structure
Local Development
Prerequisites
Environment Configuration
Starting Airflow
Using dbt
dbt Configuration
Running dbt
Connecting to Snowflake
Airflow Connection
dbt Profiles
Deploying to Astronomer
Additional Resources
1. Overview
This repository provides a quick-start template to run:

Apache Airflow for scheduling and orchestration.
dbt (data build tool) for ELT transformations in Snowflake.
Snowflake as the cloud data warehouse.
With Airflow, you can automate the process of extracting data, loading it into Snowflake, and triggering dbt transformations to keep your analytics tables up to date. Everything here is containerized via Docker, making local development straightforward.

2. Project Structure
A typical layout after running astro dev init and adding dbt resources might look like this:

makefile
Copy
Edit
├── dags/
│   └── example_astronauts.py
├── dbt/
│   ├── models/
│   ├── seeds/
│   ├── snapshots/
│   └── dbt_project.yml
├── include/
├── plugins/
├── Dockerfile
├── airflow_settings.yaml
├── packages.txt
├── requirements.txt
├── profiles/
│   └── profiles.yml
└── README.md
Below are the core directories and files to note:

dags/: Contains your Airflow DAGs (Directed Acyclic Graphs).
example_astronauts.py is a sample DAG demonstrating how to query an API and use Airflow's TaskFlow API.
dbt/: Your dbt project folder. Customize your models, seeds, snapshots, and configurations here.
profiles/: Holds the profiles.yml file that dbt uses to connect to Snowflake (and other data warehouses if needed).
Dockerfile: Builds a Docker image pinned to Astronomer’s Runtime for Airflow. Modify to add system packages or Python libraries as needed.
airflow_settings.yaml: Defines local-only Airflow Connections, Variables, and Pools (optional if you prefer the Airflow UI).
packages.txt: Lists OS-level packages for your project (if needed).
requirements.txt: Lists Python-level dependencies for your project, such as dbt-core, dbt-snowflake, etc.
plugins/: Extend Airflow’s functionality with custom or community plugins.
3. Local Development
Prerequisites
Docker Desktop or a compatible Docker installation.
Astronomer CLI.
A valid Snowflake account (for actual data operations).
Environment Configuration
Configure environment variables in your local environment or via Docker for secrets like your Snowflake user, password, and account details. You can store these in:

A .env file (if you prefer to keep them outside version control).
Airflow Connections via the airflow_settings.yaml file or the Airflow UI.
Starting Airflow
Install dependencies (optional if you have them listed in requirements.txt):

bash
Copy
Edit
pip install -r requirements.txt
Start Airflow:

bash
Copy
Edit
astro dev start
This command spins up the following Docker containers:

Postgres (Airflow Metadata Database)
Webserver (Airflow UI)
Scheduler (Orchestrates tasks)
Triggerer (Manages async/deferred tasks)
Access the Airflow UI in your web browser:

arduino
Copy
Edit
http://localhost:8080
The default credentials are admin / admin.

4. Using dbt
dbt Configuration
Navigate to the dbt directory:
bash
Copy
Edit
cd dbt
Your main dbt configuration file is dbt_project.yml. Update any project settings (name, version, models path, etc.) to match your use case.
Make sure your profiles.yml (located in the profiles/ folder or ~/.dbt/ by default) has the correct Snowflake connection details. See the dbt Profiles section for an example.
Running dbt
When inside the dbt directory, you can manually run dbt commands such as:

bash
Copy
Edit
dbt debug          # Check your connection and config
dbt compile        # Compile dbt models
dbt run            # Execute transformations
dbt test           # Test data quality
dbt docs generate  # Build documentation site
If dbt is not found, install it via:

bash
Copy
Edit
pip install dbt-core dbt-snowflake
(or confirm these packages are in requirements.txt).

5. Connecting to Snowflake
Airflow Connection
To connect Airflow tasks to Snowflake, you can define an Airflow Connection:

Go to Admin > Connections in the Airflow UI.
Create a new connection with:
Conn Id: snowflake_default (or a custom name)
Conn Type: Snowflake
Host: <your_snowflake_account>.snowflakecomputing.com
Schema: <your_schema>
Login: <your_username>
Password: <your_password>
Extra: {"account": "<your_snowflake_account>", "warehouse": "<your_warehouse>", "role": "<your_role>"}
Alternatively, add the same details to airflow_settings.yaml under connections: for local usage.

dbt Profiles
dbt uses a profiles.yml file to manage environment-specific credentials. An example profiles.yml for Snowflake might look like:

yaml
Copy
Edit
default:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: "<your_snowflake_account>"
      user: "<your_username>"
      password: "<your_password>"
      role: "<your_role>"
      warehouse: "<your_warehouse>"
      database: "<your_database>"
      schema: "<your_schema>"
      threads: 4
      client_session_keep_alive: False
Commit this cautiously if it contains credentials; consider using environment variables for secret values.

6. Deploying to Astronomer
If you have an Astronomer account and a Deployment ready:

Ensure you’re authenticated:
bash
Copy
Edit
astro login
Build and push your image:
bash
Copy
Edit
astro dev start
astro dev stop
astro deploy
Once deployed, Astronomer will build a production-grade Airflow environment and run your DAGs in the cloud.
For detailed steps, see Astronomer’s Deploy Code docs.

7. Additional Resources
Apache Airflow Docs: https://airflow.apache.org/docs
dbt Docs: https://docs.getdbt.com
Snowflake Docs: https://docs.snowflake.com
Astronomer Docs: https://docs.astronomer.io
