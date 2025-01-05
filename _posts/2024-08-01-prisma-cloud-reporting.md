---
title: "Prisma Cloud Reporting"
layout: post
date: 2024-08-01 00:00
image: /assets/images/markdown.jpg
headerImage: false
tag:
- prisma cloud
- palo alto
- kubernetes
star: false
category: blog
author: masondenney
description: Prisma Cloud
---
# Prisma Cloud Reporting
## Overview
A company might purchase a tool like Prisma Cloud because of the feature that flags vulnerabilites as "in-use" so they can prioritize remediating the vulnerabilities that matter first. The default dashboards that come with Prisma Cloud aren't very easily customizable. For example, charts like the Vulnerability Burndown chart only shows the past 30 days and cannot be changed. There is some filtering available CAGs(Compute Access Groups) but creating CAGs for different views by environment or team can be tedious. Sharing dashboards is also difficult.

With these limitations in mind, the company may choose to use the Prisma Cloud API to get the in-use vulnerability data and store it in cloud storage like buckets or databases. Once in cloud storage, tools like BigQuery can query the data easily and there are many options to visualize data. The company's developers might prefer the flexibility associated with graphing tools like Grafana or the simplicity of using BigQuery in Google Sheets.


## Building the Reporter Service
### Requirements
When designing the service, you might have the following questions:
- Why not use a image scanning tool like trivy in your pipeline?
  - That only works if images are frequently rebuilt and deployed to all environments. If images are rarely rebuilt, then the initial scan would look good but CVEs would be discovered over time. We need Prisma Cloud to check runtime via defender/Twistlock agents running on the VMs or GKE nodes.
- Why not use a cloud function to query the Prisma API?
  - We need to query the k8s master node via something like kubectl in order to get info like annotations and labels that might not be in the prisma API

Below are some requirements for the service:
- Needs to be written in Python since that is what the team uses
- Needs to report how many images have highs and critical in-use packages out of the total number of images.
- Needs to rerun/update every hour in order for devs to have quick feedback once making changes
- Needs to capture running containers as well as cronjobs that might not be currently running
- Needs to get labels and annotations from kubectl since not in prisma


### Interacting with the Prisma API
Prisma Cloud is split into several APIs, but the Prisma Cloud Compute API exposes the "in-use" flags that the defender/twistlock agents find on the nodes. The API docs are public facing and can be found at <https://pan.dev> with the Compute APIs at <https://pan.dev/compute/api/>. Each endpoint provides example usage in many different languages including Python. You will be able to find the "Get Image Scan Results" endpoint at <https://pan.dev/compute/api/get-images/> which has a response containing the "riskFactors" object that can contain the "Package In Use" string.

>WARNING: An undocumented limitation is that this endpoint cannot handle returning more than 100 results at a time


### Interacting with K8s
The two ways I retrieved data from the k8s master node was locally through kubectl and within the cluster I used the kubernetes Python library.

IN-CLUSTER EXAMPLE

```python
from kubernetes import client, config
config.load_incluster_config # for use in-cluster
v1 = client.CoreV1Api()
ret = v1.list_pod_for_all_namespaces(watch=False)
for i in ret.items:
  if i.spec.containers
    ...
  if i.spec.init_containers
    ...
```

LOCAL EXAMPLES
```python
for local use you can utilize kubetcl
os.system(f”kubectl get pods -A --context {myenv}”)
...
```

You can also use the kubernetes library locally by using 
```python
config.load_kube_config() 
```
instead of 
```python
config.load_incluster_config
```


### Creating a Contaienr
Once your python script is working it is time to make a docker container.
Start by freezing your dependencies using
```bash
python3 freeze > requirements.txt
```

Then create the Dockerfile
```Docker
FROM python-slim
RUN apt update && apt upgrade -y && apt clean
RUN mkdir /app
COPY . /app
WORKDIR /app
RUN pip install -upgrade pip && pip install -r requirements.txt
```


### Creating K8s/CNRM
In a GCP GKE environment you might end up creating the following resources via CNRM yaml:
- bucket policy
- bucket
- service account key
- service account
- workload identity (ties service account to k8s namespace)
- bigquery dataset
- bigquery table
  - Must be an external table and avoid using auto-detect for the schema 


## Visualizing the Data
### BigQuery
Service creators might want to use BigQuery to confirm the incoming data looks good. However devs might have access to the GCP Project where the data is stored so you'll likely expose the info in some other way


### Google Sheets
Google Sheets is a quick and easy way to get data displayed and to get graphs created.
In a new sheet, click on Data then click on Data Connectors then click on Connect to BigQuery
In the Connector, you will specify the GCP Project, the table and the dataset.
This will look like
```sql
SELECT * FROM `myproject.mydataset.mytable`
```
Once the Connected sheet is created you can click on the add Pivot Table button to create a new sheet with a pivot table where you can display the data in whatever way you like


### Grafana
Grafana is a popular way to build dashboards and graphs and developers may prefer to see a dashboard over a Google Sheet. To get started you will need to install the "BigQuery" plugin from <https://grafana.com/grafana/plugins/grafana-bigquery-datasource/> which will allow you to create data sources. Once installed don't forget to create the data source.
When you create a dashboard, simply use the BigQuery datasource and begin writing your SQL query like:
```sql
FROM ‘mygcpproject.mydataset.mytable’ SELECT scan date, COUNT(DISTINCT ‘image’)
...
```
Instead of just time series graphs you might want to include tables that count numbers of vulns. For that I used a query like below:
```sql
...
COUNT(CASE WHEN package_in_use = “in-use” THEN 1 END) AS in_use_count,
...
```


## Conclusion
I hope that this write up helps you create custom dashboards for "in-use" vulnerability data from Prisma Cloud.