# CDE Spark Connect QuickStarts

![alt text](img/spark-connect-slide.png)

## Objective

In this repository you can find Docker containers to help you get started with CDE Spark Connect with minimal effort. This repository will host a growing list of quickstarts including the most popular IDE's.

## Motivation

Spark Connect is a client–server architecture for Apache Spark that separates the Spark driver from the application client. It lets developers write Spark code in lightweight clients—such as Python, SQL, or even mobile and web applications—while communicating with a remote Spark session over gRPC. This design improves stability, enables long-lived interactive sessions, and makes Spark more flexible by allowing clients to upgrade independently of the Spark cluster.

Cloudera Data Engineering is a platform that helps organizations build, manage, and run Spark pipelines at scale. It provides tools for collecting, preparing, and processing large amounts of data so it can be used for analytics, reporting, or machine learning. The platform automates and simplifies Spark dependency management, job observability, resource management, and more, making it easier and faster for data engineers to scale data engineering pipelines.

You can leverage Spark Connect from anywhere.

## Requirements

To deploy the demo / HOL via this automation you need:

* A CDP tenant in Public or Private cloud.
* A CDP Workload User with Ranger policies and IDBroker Mappings configured accordingly. In particular, you must configure the Ranger Hadoop SQL and Raz (S3 or ADLS) policies accordingly.
* An CDE Service on version 1.23 or above and a CDE Spark 3.5 Virtual Cluster in the All Purpose tier.
* A working installation of Docker on your local machine.

## Contents

1. [JupyterLab CDE Spark Connect Quickstart]()
2. More to come.

## 1. JupyterLab CDE Spark Connect Quickstart

#### Pull the Docker Container and Launch JupyterLab

Launch the Docker container.

```
docker run -p 8888:8888 pauldefusco/cde_125_sparkconnect_quickstart_jl:latest
```

Launch the JupyterLab IDE in your browser by copy and pasting the provided url as shown below.

![alt text](/img/docker-container-launch.png)

You now have access to all lab materials from the JupyterLab IDE in the left pane. From here, you can launch notebooks and run the terminal.

![alt text](/img/jl-home.png)

You will use the terminal in the IDE to run the CDE CLI commands for the labs. First you need to configure the CLI and install Spark Connect though.

#### Configure the CDE CLI and Install Spark Connect for CDE.

Open CDE's configurations and apply your Workload Username and Jobs API URL. You can find your Jobs API URL in your Virtual Cluster's Details Page.

![alt text](/img/jobs-api-url-1.png)

![alt text](/img/jobs-api-url-2.png)

![alt text](/img/cli-configs-1.png)

![alt text](/img/cli-configs-2.png)

Next, generate a CDP access token and edit your CDP credentials.

![alt text](/img/usr-mgt-1.png)

![alt text](/img/usr-mgt-2.png)

![alt text](/img/usr-mgt-3.png)

![alt text](/img/cdp-credentials.png)

Finally, run the following commands to install the CDE Spark Connect tarballs.

```
pip3 install cdeconnect.tar.gz  
pip3 install pyspark-3.5.4.tar.gz
```

![alt text](/img/install-deps.png)

#### Upload data to CDE Files Resource

Compress files for upload:

```
tar -czvf rawtransactions.tar.gz rawtransactions
tar -czvf trx_batch_1.tar.gz trx_batch_1
tar -czvf trx_batch_2.tar.gz trx_batch_2
```

Create a CDE Files Resource and upload files:

```
cde resource create \
  --name data \
  --type files

cde resource upload-archive \
  --name data \
  --local-path rawtransactions.tar.gz \
  --local-path trx_batch_1.tar.gz \
  --local-path trx_batch_2.tar.gz
```

![alt text](/img/compress.png)

#### Launch a CDE Spark Connect Session

Start a CDE Session of type Spark Connect. Edit the Session Name parameter so it doesn't collide with other users' sessions. You will be prompted for your Workload Password. This is the same password you used to log into CDP.

```
cde session create \
  --name jl-qs-session \
  --type spark-connect \
  --num-executors 2 \
  --driver-cores 2 \
  --driver-memory "2g" \
  --executor-cores 2 \
  --executor-memory "2g" \
  --mount-1-resource data
```

![alt text](/img/launchsess.png)

In the Sessions UI, validate the Session is Running.

![alt text](/img/cde_session_validate_1.png)

![alt text](/img/cde_session_validate_2.png)

#### Run the Notebook Cells

Now that the session is running you're ready to run the code. Make sure your session name (in this example ```jl-qs-session```) reflects the argument to the ```sessionName``` constructor in the first cell.

![alt text](/img/notebook-screenshot.png)

## Summary & Next Steps

Spark Connect introduces a modern client–server architecture that cleanly separates applications from the Spark driver, allowing developers to interact with Spark from lightweight clients such as JupyterLab, web applications, microservices, or even mobile apps.

Because the client communicates with the Spark session over a stable gRPC interface, developers can build flexible, long-lived workflows without needing a full Spark installation locally.

When used with Cloudera Data Engineering (CDE), Spark Connect allows notebooks, scripts, and custom applications to leverage a fully managed, scalable Spark cluster for development, interactive exploration, and production-grade pipelines — all while keeping the client environment simple, portable, and easy to iterate on.

## Useful docs & blogs

* **External IDE connectivity through Spark Connect-based sessions (CDE docs)** — official documentation on how to configure a Spark Connect session in CDE, including prerequisites and limitations. ([Cloudera Docs][2])
* **Elevating Productivity: Cloudera Data Engineering Brings External IDE Connectivity to Apache Spark** — blog post describing how CDE’s support for external IDEs (Jupyter, PyCharm, VS Code) improves developer productivity. ([Cloudera][3])
* **Managing Python dependencies for Spark workloads in Cloudera Data Engineering** — useful notes on how to handle Python dependencies in CDE when running Spark jobs, which is relevant when your notebook environment differs from cluster environment. ([Cloudera][4])
* **SWAN / Jupyter + Spark Connect discussion (Spark community)** — a write-up describing how Spark Connect can integrate with Jupyter (or other web-based notebook environments) to allow a client-server model for Spark computation. ([hepsoftwarefoundation.org][5])


[1]: https://community.cloudera.com/t5/Community-Articles/JupyterLab-and-Spark-Connect-Quickstart-in-Cloudera-Data/ta-p/400298?utm_source=chatgpt.com "JupyterLab and Spark Connect Quickstart in Clouder... - Cloudera Community - 400298"
[2]: https://docs.cloudera.com/data-engineering/1.5.5/sessions/topics/cde-spark-connect-session.html?utm_source=chatgpt.com "External IDE connectivity through Spark Connect-based sessions (Technical Preview)"
[3]: https://www.cloudera.com/blog/technical/elevating-productivity-cloudera-data-engineering-brings-external-ide-connectivity-to-apache-spark.html?utm_source=chatgpt.com "Elevating Productivity: Cloudera Data Engineering Brings External IDE Connectivity to Apache Spark | Blog | Cloudera"
[4]: https://blog.cloudera.com/managing-python-dependencies-for-spark-workloads-in-cloudera-data-engineering/?utm_source=chatgpt.com "Managing Python dependencies for Spark workloads in Cloudera Data Engineering | Cloudera Blog"
[5]: https://hepsoftwarefoundation.org/gsoc/2023/proposal_SWAN_SparkConnect.html?utm_source=chatgpt.com "Leverage Spark Connect for interactive data analysis in Jupyter Notebooks"
