*** WORK IN PROGRESS ***

# SolrCloudMonitoring
This tool helps monitor Solr Cloud

Table of Content

1. Overview

        1.1. Introduction
        
        1.2. Advantage
        
        1.3. Scope of monitoring
        
        1.4. Quick Installation
        
2. Tool technical overview

        2.1. Tool Directory layout
        
        2.2. Execution Process
        
        2.3. Configuration and History Management overview
        
        2.4. Toubleshooting steps
        
        2.5. Incident Management Tool Integration Process
        
3. Property file overview and details of property monitored

4. Support and managebility

## 1. Overview

### 1.1. Introduction

Single we do not have a tool to monitor a secured (Kerberos) Solr Cloud. This tool of mine help you monitor your Solr Cloud. This tool has the ability to detect and report issues in your Solr Cloud. Note the tool works per Collection basis monitoring thus you will have to execute separate instances for different Collections in your Solr Cloud.

### 1.2. Advantage

The tool does different type of validation and gives you a consolidated health report of your Solr Cloud. Below are some of the advantages.

        a. Monitor Secured Solr Cloud Cores. Reports if any has a non active state.
        b. Monitor query response of your Cloud. You can set your expected response time threshold.
        c. Since the tool works per Collection thus giving you flexibility to monitor only the collection you you want to monitor(critical for your environment).

### 1.3. Scope of monitoring

At this point of time the tool can be used to monitor any Solr Cloud (but currently being tested in a CDH Production Cluster). The tool relies heavily on Solr Admin API to monitor the Cloud. The tool can monitor a CDH Environment with SSL Secured API which is protected with User Authntication(Kerberos).

Since tool is Unix Bash based it is only limited to Unix environment and it has been tested on Linux cluster.

### 1.4. Quick Installation

This probably is the best part of the tool. The installation require minimum effort, but before you install please make sure the below is available in your environment.

        a. Utility that should be present are quite basic core Linux utility mostly comes with Default Linux build. Eg: curl, netstat.
        b. SMTP Setup is required for the tool to send Alerts for issue detection.
        
The installation require the tools directory to be placed on any location on the host where the tool need to be executed. This is all it takes to install the tool. But please do go though the Tool technical overview section to understand how the tool should be setup for monitoring and finally initiate the actual monitiong. 

## 2. Tool technical overview



### 2.1. Tool Directory layout


[DIRECTORY] **tmp** - Temporary work area. Files inside it can be deleted after execution but not during execution.

[DIRECTORY] **log** - Log directory. 

[DIRECTORY] **datastore** - Keytab file location. In case your cluster is secure and you will have to place the tool users Keytab file here, name should be [User Name].keytab

[DIRECTORY] **history** - Contains report file for Incident Management integration. 

[FILE] **solr_monitor.prop** - Main property file. This need to be filled for the tool to be operational.

[FILE] **solr_monitor** - Monitoring script. 

[FILE] **scheduler** - Main executor script. This is to be executed from the monitoring.

### 2.2. Execution Process

The tool can be executed by

[SCRIPT HOME]/scheduler [Collection Name]

***NOTE:*** Its imporatant to understand that just executing the monitor will not allow you to continuously monitor the environment and we should setup some kind of repeated execution mechanism via your Enterprise Scheduler, e.g. ControlM. As a sample setup the below example will help you execute the monitor in a periodically basis in a ***once every 3 hours*** using Unix default scheduler Crontab.

> `* */3 * * * [SCRIPT HOME]/scheduler [collection_name] >> [SCRIPT HOME]/log/monitor.cron.log 2>> [SCRIPT HOME]/log/monitor.cron.log

### 2.4. Toubleshooting steps

The script has been build to log fairly verbose log level, so that any issue if occured can be identified using the log only. The log for the two execution type supported is stated below:

SCOPE | LOCATION | LOG NAME
--- | --- | ---
[collection_name] | log | **[collection_name].solr_monitor.log**

***NOTE*** The tool keeps the log file to a specified size ([PROPERTY] solr.monitor.logsizemb) in MB and autorotate keeping one additional backup file.

### 2.5. Incident Management Tool Integration Process

The tool can be easily intergrated with your current enterprise monitoring system to generate Incidents in your standard Enterprise Incident Management system. The below mechanism should be followed for the integration.

* Enable Log Watcher module in your enterprise monitoring system on the monitoring host.
* Enable reporting in tool by setting configuration value solr.monitor.report to true/TRUE.
* Configure Log Watcher to search for pattern ***:ERROR:*** in report file inside history directory.


## 3. Property file overview and details of property monitored

The propery file is the key to configure monitoring for your environment. The blank property file ***solr_monitor.prop*** is extensively commented to explain each and every property in quite detail fashion along with sample value.


## 4. Support and managebility

If you are reading this README file then you are probably about to use the my tools to help you monitor your Hadoop Cluster. Good choice. This tool is made for you. Moreover this tool is free and always will be thats my promise.

Now it is hard to believe that you will get 24/7 Support thats too much to ask for. But in case you face any issue and want my intervention and you cannot debug the hundreeds lines of core Bash Script your self, please do not hassitate to write to me. Its a guarentee you will get an answer but it is not a guarentee you will have it in a SLA.

Reach Me: sanmuk21@gmail.com

Best of luck. Happy Monitoring your Solr Cloud.
