# Rapidly Ingest, Transform, Query and Act on Your Streaming Data in Microsoft Fabric - Leaving Las Vegas 

## Table of content
- [Context of the data](#context-of-the-data)
- [Environment setup](#environment-setup)
    - [Create a parameter file](#create-a-parameter-file)
    - [Create a Lakehouse](#create-a-lakehouse)
    - [Save the File Paths](#save-the-abfs-path-for-the-sample-file)
- [Create a KQL Database](#create-a-kql-database)
- [Create a KQL Query set](#create-a-kql-query-set)
    - [Structure your KQL Query set](#structure-your-kql-query-set)
    - [Explore the data](#explore-the-data)
- [Build the Event Stream](#build-the-event-stream)
    - [Insert the variables in the notebook](#insert-the-variables-in-the-notebook)
    - [Run the notebook and confirm the data is streaming](#run-the-notebook-and-confirm-the-data-is-streaming)
    - [Create an event stream KQL database destination](#create-an-event-stream-kql-database-destination)
    - [Configure an additional destination to demonstrate filtering](#configure-an-additional-destination-to-demonstrate-filtering)
    - [Configure a Reflex output to trigger based on latitude](#configure-a-reflex-output-to-trigger-based-on-latitude)
- [Build the Power BI report](#build-the-power-bi-report)
- [Prepare for the first run](#prepare-for-the-first-run)
    - [Empty the main tables](#empty-the-main-tables)  
    - [Start the notebook](#start-the-notebook)
    - [Watch the report!](#watch-the-report)  



## Context of the data
The OpenSky network is a worldwide group of volunteer plane watchers using ADS-B receivers to track the transponder signals emitted by the civil aviation traffic in range of their receiver. The sum total of all the receivers builds a worldwide history of aircraft tracking. The OpenSky Network allows the use of daily extracts of this data for research and learning purposes. Please take great care to read the terms of use and licence.txt files. Abiding by these rules is what allows rich datasets like these to continue being available to us to use with training and learning.

In the interest of time, we're going to be using a special subset of this massive dataset so we can ensure a smooth experience.

[^ Table of content ^](#table-of-content)

## Environment setup
### Create a parameter file
Throughout this workshop, we will create many services that need to talk to each other. To that end we will need to capture endpoints url, file paths and many more parameters. We suggest that at this point you start using a parameter file to capture the information when creating the services, so you don't have to hunt down these parameters later down the line.

[^ Table of content ^](#table-of-content)

### Create a Lakehouse 
1. In the Fabric experience picker at the bottom left of the screen, select Data Engineering
1. In the section New click on the button LakeHouse
1. Name your lakehouse **LeavingLasVegas_lh**
1. Create two folders:
    1. ***Reference*** FlightInformationRegions.tsv.gz goes here
    1. ***SampleData*** NKS flights.csv.gz goes here
1. In the Lakehouse menu, click the ellipsis next to the ***“Reference”*** folder and click ***“Upload”*** according to the above structure
1. Use the folder button to navigate to where you saved the two data files located in the ***Data*** folder of this repo on your local machine and hit Open. 
1. Click Upload
1. Repeat this for both files going to their respective folders

[^ Table of content ^](#table-of-content)

### Save the ABFS path for the sample file
1. ***Right click*** on each of the file file in the lakehouse and hit ***Properties***
1. Use the ***copy button*** next to the ABFS filepath
1. ***Paste the ABFS paths*** for both data files to your ***Param file***

[^ Table of content ^](#table-of-content)

## Create a KQL Database
Open the Microsoft Fabric Webpage

Ensure you are in the ***real time analytics experience*** by clicking the ***experience picker*** in the lower left corner and selecting Real-Time Analytics

1. Click the ***"KQL database"*** button at the top of the screen
1. Name your database: ***LeavingLasVegas_kdb***
1. On the main page of the KQL Database, note the Query URI with a dedicated button to copy the URI. Click this button and **Save the Query URI AND the Database name to your Param file** .

[^ Table of content ^](#table-of-content)

## Create a KQL Query set
Navigate back to My workspace using the shortcut to the left of the screen

Click the New dropdown menu to the top left of the screen and select ***KQL Query set***

1. Name your query set: ***LeavingLasVegas_kqs*** and click on ***Create***.
1. Select the ***LeavingLasVegas_kdb*** Database and click on ***Connect***

[^ Table of content ^](#table-of-content)

### Structure your KQL Query set
Inside your KQL Queryset, create three additional tabs using the + sign next to the first tab.

1. Name your first tab ***LLV_DDL***
1. Name your second tab ***LLV_Explore***
1. Name your third tab ***LLV_Func***
1. Name your third tab ***LV_Cleanup***

From this repo **copy and paste the content of the KQL scripts found in the repo to their corresponding tabs in the KQL Queryset**

Note that in the llv_ddl code, there are direct ingestion commands embedded. ***They need to be updated with the correct file path from your param file*** 

[^ Table of content ^](#table-of-content)

### Prepare the Database structure
While still inside your KQL Query set, navigate to the ***llv_ddl*** tab and ***run the code block***. 

***Please allow for a few minutes*** to pass as this code not only creates the tables and objects, it will ingest reference data and perform some important transformations.

Next click the ***llv_func*** tab and ***run that code block*** as well. This will create important views and tables to help with visualizations.

[^ Table of content ^](#table-of-content)

### Explore the data

At this point we have the data already loaded. We will delete it all soon so we can stream it in for the full experience but now is a good time to look at some cool KQL!

Click on the ***llv_explore*** tab and let’s run each code block to see what insight they produce! 
## Deploy the Stream simulator
Download the ***LeavingLasVegas_nb.jpynb*** file from this repo and save it to your local machine
1. Navigate back to the ***Data engineering landing*** page by using the experience picker at the bottom left corner of the screen.
1. Click the ***Import Notebook button***.
1. and click the ***Upload*** button
1. Navigate to your local file system where you saved the notebook file and hit Open

[^ Table of content ^](#table-of-content)

## Build the Event Stream
1. Navigate to the ***Real-time Analytics landing page*** using the experience picker
1. Click the ***Event Stream button*** at the top and wait for the naming prompt
1. Name your event stream: ***LeavingLasVegas_ES***
1. Click the ***New source button*** and select ***Custom app***
1. Name this source ***llv_CA_Source***
1. With the source selected click the ***Keys*** button and click the ***eye button*** to the left of the ***Connection string-primary key*** element to display the full string
1. Use the ***Copy button*** to copy and Paste the URI to your to your Param file.

[^ Table of content ^](#table-of-content)

### Insert the variables in the notebook
1. Navigate to the Data Engineering landing page using the experience picker
1. Navigate back to the LeavingLasVegas_Notebook you uploaded earlier
1. In the cell at the top you will find the EventStreamEndpoint variable that should be populated with the Custom app endpoint you created ealier and saved to your param file.
1. Next is the OpenSkyDataFolder variable that you should populate with the Sample data folder ABFS path that you captured earlier.

[^ Table of content ^](#table-of-content)

### Run the notebook and confirm the data is streaming

We will now be ready to run the notebook and stream the data to event stream. You may remember that we created a source but no destination. The reason is simple, we have no data in the pipe yet and the KQL db destination of event stream must see some of it to allow its creation. So we will stream it now but let's remember that this will be an incomplete stream and we'll have to do it all again later.

1. Click Run all at the top.
1. Allow for the spark session to spin up and watch for logs at the bottom that will indicate that the stream has started. you should be seeing messages like this:

*This was batch #:136
We loaded rows from: 1620 to row: 1632
Rows remaining in the stream: 134*

[^ Table of content ^](#table-of-content)

## Create an event stream KQL database destination
With the stream active, navigate back to your ***LeavingLasVegas_ES*** Event Stream.
1. Click the ***New Destination*** button and select ***KQL Database***
1. Select ***Direct Ingestion***
1. Name your destination ***llv_KQLDb_Dest***
1. Select the workspace you are working in. (e.g. "My Workspace")
1. Select the ***LeavingLasVegas*** KQL database and hit ***Add and Configure***
1. Now you must ***complete the data mapping***.
1. Click the ***LeavingLasVegas*** table.
1. Leave the connection name as is or choose your own and hit Next
1. On the next screen immediatly ***switch the format to JSON*** and hit the ***advanced button*** to the right
1. Select ***Existing mapping*** and choose the ***LeavingLasVegas_json_mapping*** element in the drop down.
1. Click ***Finish***

[^ Table of content ^](#table-of-content)

### Configure an additional destination to demonstrate filtering
Create another KQL destination 

1.	In ***LeavingLasVegas_ES*** Eventstream, click the ***New Destination*** and select ***KQL Database***.
2.	Select ***Event processing before ingestion***
3.	Name your destination ***llv_KQLDb_Dest2***
4.	Select the workspace you are working in. (e.g. "My Workspace")
5.	Select the ***LeavingLasVegas KQL database***.
6.	Select the table ***noNKS71***.
7.	Select ***Open Event processor***.
8.	Hover on your Eventstream, and click on ***‘+’*** and then select the operation ***‘filter’***.
9.	Configure the filter operator on the right side by defining the following conditions:
    - Select ***field = CallSign***
    - ***Does not equal NKS71***
    - This filters out events where CallSign is not NKS71
10.	Click ***Done***.
11.	Select ***KQL Database node*** in this event processing editor and check ***Data preview*** in the bottom pane.
12.	Close Event processing editor.
13.	Click ***Add*** on KQL database pane and enter your KQL DB details, the Table “NoNKS71” should already exist as part of the DDL creation script.
14.	In Details tab of KQL database node, you will be able to see many details like Data preview, Data insights. You can also open KQL Database from here.

[^ Table of content ^](#table-of-content)

### Configure a Reflex output to trigger based on latitude
Here we will create a data activator reflex that will trigger whenever a flight crosses latitude 30 degrees North heading south. In other words, the latitude will become less than 30 meaning we’re heading towards the equator.

15.	In ***LeavingLasVegas_ES*** Eventstream, click the ***New Destination*** and select ***Reflex***.
16.	Name your destination ***llv_Reflex_Dest***
17.	Select the workspace you are working in.
18.	Create a ***new Reflex*** and name it ***Reflex_Lat***.
19.	Select ***Save***.
20.	Open the Reflex that you just created. Using the ***Open item*** link
Give Object name ***Lat_Notify***. Select key column as ***FlightTrackLat***.
21.	Click ***Save***. And then ***Save and go to design mode***.
22.	Click on ***New Trigger***. And select ***FlightTrackLat***
23.	Choose ***Detect condition*** to be Becomes ***Less than*** and specify value ***30***.
24.	Select ***Action*** as ***Teams message***.
25.	***Set your Headline message*** to ***“Flight number @Callsign, breached latitude 30 degree at @timeStamp”*** .
26.	***Save*** & ***Start*** the trigger.

[^ Table of content ^](#table-of-content)

## Build the Power BI report
***Download the Power BI template document*** (.PBIT) from the repo here and Save it to your local drive. If you cloned the Git Hub Repository you will find the Power BI Report in folder Code/PowerBI.

1. ***Open the PBIT file*** and wait for it to ***prompt you for the parameters***
1. It should prompt you for ***two parameters***
1. ***MyQueryUri*** is the URI of your KQL db that you saved in your param file.
1. ***MyDatabaseName*** is the name you chose originally for the database.
1. Click ***Load*** and allow the report to connect and load data
1. It may ***require you to sign in to authenticate*** if not done already
1. ***Publish*** the report to your workspace

[^ Table of content ^](#table-of-content)

## Prepare for the first run
Time to get ready for the first run! We must do some cleaning first
### Empty the main tables
1. Navigate back to your KQL query set and click the **lvl_cleanup tab**, 
1. Execute the ***first code block*** to clear the two main table. ***DO NOT*** execute the second code block as this will empty the reference table.
### Start the notebook
1. Navigate back to the ***LeavingLasVegas_nb*** notebook and click ***“Run"***

[^ Table of content ^](#table-of-content)

### Watch the report!
When you see the logs at the bottom of the notebook indicating the data is streaming, quickly navigate to the LeavingLasVegas_rpt report and watch the magic happen!

[^ Table of content ^](#table-of-content)