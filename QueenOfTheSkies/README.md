# Queen of the skies demo
This demo will allow you to leverage the elements of Fabric Real-Time Analytics to showcase a powerful demo with a compeling story.

## Demo backstory
On February 1st, 2023, something very special happened in Everett, WA. Atlas Air, a well-known cargo plane operator, took delivery of a new airplane. Only this plane happens to be the last of the Boeing 747 that will ever be made.

## Table of content

- Prepare a text file for your important parameters
- Create a KQL database and KQL Queryset
   - Run the database preparation script
   - Ingest the sample file
   - Rehydrate the planned flight table
   - Prepare for the first run
- Create a Lakehouse
- Upload the notebook
   - Run the notebook and confirm the data is streaming
- Create the event stream
   - Create a custom app source
   - Create a KQL database destination
      - Complete the data mapping
- Generate the report using the Power BI report template
- Run the demo
   - Prepare the landing table
   - Run the preflight check
   - Start the notebook
   - Run the report

## Prepare a text file for your important parameters
We will need to capture many parameters that will ne needed later in this project. We will ask you to save this in a text file as you follow along.

## Create a KQL database and KQL Queryset

1. Ensure you asre in the real time analytics experience by clicking the experience picker in the lower left corner and selecting Real-Time Analytics
1. Click the "KQL database" button at the top of the screen
1. Name your database: ***QueenOfTheSkies*** 
1. On the main page of the KQL Database, note the Query URI with a dedicated button to copy the URI. Click this button and **Save the Query URI AND the Database name to your Param file.**
1. Navigate back to ***My workspace*** using the shortcut to the left of the screen
1. Click the **New** dropdown menu to the top left of the screen and select ***KQL Query set*** 
1. Name your query set: ***Qos_KQS***

### Structure your KQL Query set
1. Inside your KQL Queryset, create **two additional tabs** using the ***+*** sign next to the first tab.
   1. Name your first tab ***QoS DDL***
   1. Name your second tab ***QoS Planned***
   1. Name your third tab ***QosDemo control***
1. From this repo copy and paste the content of the KQL scripts found in [the repo](/Code/KQL/) to their corresponding tabs in the KQL Queryset

### Run the database preparation script

1. Run the code contained in the ***QoS DDL*** script
1. Ensure all operation are successful

### Ingest the sample file
We now will ingest the full flight path all at once. We do this so we can use the complete dataset to reconstruct the planned flight by surgically removing the section of the flight path we do not want and inferring the *planned* data points. In anticipation of that, please download the [***QueenOfTheSky_ex.csv***](/Data/QueenOfTheSky_ex.csv) file from this repo and save it to your local machine.

1. Navigate back to the ***QueenOfTheSKies*** KQL database.
1. Click on the down chevron next to the ***Get Data*** button and select ***Local file***
1. In the next screen select the ***QueenOfTheSky*** table and hit the ***Browse for files*** button.
1. Navigate to where you saved the ***QueenOfTheSky_ex.csv*** file on your local machine and hit ***Open*** 
1. Let the file upload and hit ***Next***
1. On the next screen, click ***Advanced**, select the ***Use existing mappings*** and select the ***QueenOfTheSky_mapping*** element in the drop down.
1. Hit ***Finish** and ensure the ingestion completes successfully.

### Rehydrate the planned flight table

1. Navigate back to your ***Qos_KQS*** KQL queryset
1. Navigate to the ***QoS Planned*** tab
1. Run the code and it should display a set of table statistics showing you have indeed ingested data in the ***QueenOfTheSkyPlanned*** table

### Prepare for the first run

The next steps will create the lakehouse where we will host the sample file and create the artefacts we need to simulate the stream

## Create a Lakehouse
1. In the Fabric experience picker at the bottom left of the screen, select ***Data Engineering***
1. Click the ***New** drop down menu and select ***LakeHouse***
1. Name your lakehouse ***QoS_LH***

### Upload the sample file

1. In the Lakehouse menu, ***Right click*** on ***Files*** and then select ***Upload*** and finally ***Upload files***
1. Use the folder button to navigate to where you saved the ***QueenOfTheSky_ex.csv*** file on your local machine and hit ***Open*** 
1. Click ***Upload***

### Save the ABFS path for the sample file
1. ***Right click*** on the  ***QueenOfTheSky_ex.csv*** file in the lakehouse and hit ***Properties***
1. Use the copy button next to the ***ABFS filepath**
1. **Paste the ABFS path to your to your Param file.**

## Upload the notebook

1. Download the [***QueenOfTheSkies_Notebook.jpynb***](/Code/Notebooks/QueenOfTheSkies_Nb.ipynb) file from this repo and save it to your local machine
1. Navigate back to the ***Data engineering landing page*** by using the experience picker at the bottom left corner of the screen.
1. Click the ***Import Notebook*** button and click the ***Upload*** button
1. Navigate to your local file system where you saved the notebook file and hit ***Open***

## Create the event stream

1. Navigate to the ***Real-time Analytics*** landing page using the experience picker
1. CLick the ***Event Stream*** button at the top and wait for the naming prompt
1. Name your event stream: ***QoS_ES*** 

### Create a custom app source

1. Click the ***New source*** button and select ***Custom app***
1. Name this source ***QoS_CA_Source***
1. With the source selected click the ***Keys*** button and click the *eye* button to the left of the ***Connection string-primary key*** element to display the full string
1. Use the ***Copy button*** to copy and ***Paste the URI to your to your Param file.***

### Insert the variables in the notebook

1. Navigate back to the ***ueenOfTheSkies_Notebook*** you uploaded earlier
1. In the cell at the top you will find the ***MyConnectionString*** variable that should be populated with the Custom app endpoint you created ealier and saved to your param file.
1. Next is the ***SampleCsv*** variable that you should populate with the Sample file ABFS path that you captured earlier.

### Run the notebook and confirm the data is streaming

We will now be ready to run the notebook and stream the data to event stream. You may remember that we created a source but no destination. The reason is simple, we have no data in the pipe yet and the KQL db destination of event stream has to see some of it to allow its creation. So we will stream it now but let's remember that this will be an incomplete stream and we'll have to do it all again later.

1. Navigate back to the ***ueenOfTheSkies_Notebook*** 
1. Click ***Run all*** at the top.
1. Allow for the spark session to spin up and watch for logs at the bottom that will indicate that the stream has started. you should be seeing messages like this: *This was batch #:136
We loaded rows from: 1620 to row: 1632
Rows remaining in the stream: 134*

### Create a KQL database destination

1. With the stream active, navigate back to your ***QoS_ES*** Event Stream.
1. Click the ***New Destination*** button and select ***KQL Database***
1. Select ***Direct Ingestion*** 
1. Name your destination ***QoS_KQLDb_Dest***
1. Select the workspace you are working in
1. Select the ***QueenOfTheSkies*** KQL database and hit ***Add and Configure***


#### Complete the data mapping

Now you must complete the data mapping.

1. Click the ***QueenOfTheSky*** table 
1. Leave the connection name as is or choose your own and hit ***Next***
1. On the next screen immediatly switch the format to ***JSON*** and hit the *** advanced button***
1. Select ***Existing mapping** and choose the ***QueenOfTheSky_mapping*** element in the drop down.
1. Click ***Finish***

**Congratulation!** you are now able to stream any CSV file to a KQL database!

## Build the Power BI report

1. Download the Power BI template document (.PBIT) from the repo here and ***Save it to your local drive***
1. Open the PBIT file and wait for it to prompt you for the parameters
1. It should prompt you for two parameters
   1. ***MyQueryUri*** is the URI of your KQL db that you saved in your param file. 
   1. ***MyDatabaseName*** is the name you chose originally for the database.
1. Click ***Load*** and allow the report to connect and load data
1. It may require you to sign in to authenticate if not done already

## Run the demo

### Prepare the landing table

### Run the preflight check

### Start the notebook

### Run the report