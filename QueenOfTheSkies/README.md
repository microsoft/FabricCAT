# Queen of the skies demo

This demo will allow you to leverage the elements of Fabric Real-Time Analytics to showcase a powerful demo with a compeling story.

## Demo backstory

On February 1st, 2023, something very special happened in Everett, WA. Atlas Air, a well-known cargo plane operator, took delivery of a new airplane. Only this plane happens to be the last of the Boeing 747 that will ever be made.

## Table of content

- [Prepare a text file for your important parameters](#prepare-a-text-file-for-your-important-parameters)
- [Create a KQL database and KQL Queryset](#create-a-kql-database-and-kql-queryset)
  - [Run the database preparation script](#run-the-database-preparation-script)
  - [Ingest the sample file](#ingest-the-sample-file)
  - [Rehydrate the planned flight table](#rehydrate-the-planned-flight-table)
  - [Prepare for the first run](#prepare-for-the-first-run)
- [Create a Lakehouse](#create-a-lakehouse)
- [Upload the notebook](#upload-the-notebook)
  - [Run the notebook and confirm the data is streaming](#run-the-notebook-and-confirm-the-data-is-streaming)
- [Create the event stream](#create-the-event-stream)
  - [Create a custom app source](#create-a-custom-app-source)
  - [Create a KQL database destination](#create-a-kql-database-destination)
    - [Complete the data mapping](#complete-the-data-mapping)
- [Build the Power BI report](#build-the-power-bi-report)
- [Run the demo](#run-the-demo)
  - [Prepare the landing table](#prepare-the-landing-table)
  - [Run the preflight check](#run-the-preflight-check)
  - [Start the notebook](#start-the-notebook)
  - [Run the report](#run-the-report)

## Prepare a text file for your important parameters

We will need to capture many parameters that will ne needed later in this project. We will ask you to save this in a text file as you follow along.

[^ Table of content ^](#table-of-content)

## Create a KQL database and KQL Queryset

1. Open the [Microsoft Fabric Webpage](https://app.fabric.microsoft.com/home)
2. Ensure you are in the real time analytics experience by clicking the experience picker in the lower left corner and selecting Real-Time Analytics

   ![Real time analytics experience](./images/image-001.png)

3. Click the "KQL database" button at the top of the screen

   ![KQL Database button](./images/image-002.png)

4. Name your database: ***QueenOfTheSkies*** 

   ![name database](./images/image-003.png)

5. On the main page of the KQL Database, note the Query URI with a dedicated button to copy the URI. Click this button and **Save the Query URI AND the Database name to your Param file.**

   ![copy URI](./images/image-004.png)

6. Navigate back to ***My workspace*** using the shortcut to the left of the screen

   ![back to my workspace](./images/image-005.png)

7. Click the **New** dropdown menu to the top left of the screen and select ***KQL Query set*** 

   ![Create KQL Query Set](./images/image-006.png)

8. Name your query set: ***Qos_KQS*** and click on **Create**.

   ![Name query set](./images/image-007.png)

9. Select the ***QueenOfTheSkies*** Database and click on **Connect**

   ![Select database](./images/image-008.png)

### Structure your KQL Query set

1. Inside your KQL Queryset, create **two additional tabs** using the ***+*** sign next to the first tab.
   1. Name your first tab ***QoS DDL***
   2. Name your second tab ***QoS Planned***
   3. Name your third tab ***QosDemo control***

      ![Insert KQL Query set](./images/image-009.png)

2. From this repo copy and paste the content of the KQL scripts found in [the repo](./Code/KQL/) to their corresponding tabs in the KQL Queryset

      ![copy content](./images/image-010.png)

### Run the database preparation script

1. Run the code contained in the ***QoS DDL*** script

   ![run code](./images/image-011.png)

2. Ensure all operation are successful

      ![check all operations are successful](./images/image-012.png)

### Ingest the sample file

We now will ingest the full flight path all at once. We do this so we can use the complete dataset to reconstruct the planned flight by surgically removing the section of the flight path we do not want and inferring the *planned* data points. In anticipation of that, please download the [***QueenOfTheSky_ex.csv***](/Data/QueenOfTheSky_ex.csv) file from this repo and save it to your local machine.

1. Navigate back to the ***QueenOfTheSKies*** KQL database.
   1. At the left bottom in the menu click on ***Real-Time Analytics***

      ![click Real-Time Analytics](./images/image-013.png)

   2. Then click on the KQL database ***QueenOfTheSkies***

      ![Select database QueenOfTheSkies](./images/image-014.png)

2. Click on the down chevron next to the ***Get Data*** button and select ***Local file***

   ![Click Get Data / local file](./images/image-015.png)

3. In the next screen select the ***QueenOfTheSky*** table and hit the ***Browse for files*** button.

   ![Browse for files](./images/image-016.png)

4. Navigate to where you saved the ***QueenOfTheSky_ex.csv*** file on your local machine and hit ***Open***

   ![Open file](./images/image-017.png)

5. Let the file upload and hit ***Next***

   ![Click next](./images/image-018.png)

6. On the next screen, click ***Advanced**, select the ***Use existing mappings*** and select the ***QueenOfTheSky_mapping*** element in the drop down.

   ![select mappiing](./images/image-019.png)

7. Hit ***Finish***

   ![hit finish](./images/image-020.png)

8. Ensure the ingestion completes successfully and click ***Close***

   ![click close](./images/image-021.png)

### Rehydrate the planned flight table

1. Click on ***Real-Time Analytics*** in the bottom left menu.

   ![click on Real-Time Analytics](./images/image-022.png)

2. Select the ***Qos_KQS*** KQL queryset

   ![Select queryset](./images/image-023.png)

3. Navigate to the ***QoS Planned*** tab

   ![Navigate to QoS Planned tab](./images/image-024.png)

4. Run the code and it should display a set of table statistics showing you have indeed ingested data in the ***QueenOfTheSkyPlanned*** table

   ![run query](./images/image-025.png)

### Prepare for the first run

The next steps will create the lakehouse where we will host the sample file and create the artefacts we need to simulate the stream

[^ Table of content ^](#table-of-content)

## Create a Lakehouse

1. In the Fabric experience picker at the bottom left of the screen, select ***Data Engineering***

   ![pick data engineering](./images/image-026.png)

2. In the section ***New*** click on the button ***LakeHouse***

   ![select new lakehouse](./images/image-027.png)

3. Name your lakehouse ***QoS_LH***

   ![name your lakehouse](./images/image-028.png)

### Upload the sample file

1. In the Lakehouse menu, ***Right click*** on ***Files*** and then select ***Upload*** and finally ***Upload files***

   ![upload files](./images/image-029.png)

2. Use the folder button to navigate to where you saved the ***QueenOfTheSky_ex.csv*** file on your local machine and hit ***Open***

   ![select local file](./images/image-030.png)

3. Click ***Upload***

   ![click upload](./images/image-031.png)

### Save the ABFS path for the sample file

1. ***Right click*** on the  ***QueenOfTheSky_ex.csv*** file in the lakehouse and hit ***Properties***

   ![click properties](./images/image-032.png)

2. Use the copy button next to the ***ABFS filepath***

   ![copy abfs path](./images/image-033.png)

3. **Paste the ABFS path to your to your Param file.**

[^ Table of content ^](#table-of-content)

## Upload the notebook

1. Download the [***QueenOfTheSkies_Notebook.jpynb***](/Code/Notebooks/QueenOfTheSkies_Notebook.ipynb) file from this repo and save it to your local machine

2. Navigate back to the ***Data engineering landing page*** by using the experience picker at the bottom left corner of the screen.

   ![select local file](./images/image-034.png)

3. Click the ***Import Notebook*** button.

   ![select local file](./images/image-035.png)

4. and click the ***Upload*** button

   ![select local file](./images/image-036.png)

5. Navigate to your local file system where you saved the notebook file and hit ***Open***

   ![select local file](./images/image-037.png)

[^ Table of content ^](#table-of-content)

## Create the event stream

1. Navigate to the ***Real-time Analytics*** landing page using the experience picker

   ![navigate to Real-Time Analytics page](./images/image-038.png)

2. CLick the ***Event Stream*** button at the top and wait for the naming prompt

   ![click on Event Stream](./images/image-039.png)

3. Name your event stream: ***QoS_ES***

   ![click on Event Stream](./images/image-040.png)

### Create a custom app source

1. Click the ***New source*** button and select ***Custom app***

   ![click on New source](./images/image-041.png)

2. Name this source ***QoS_CA_Source***

   ![name the source](./images/image-042.png)

3. With the source selected click the ***Keys*** button and click the *eye* button to the left of the ***Connection string-primary key*** element to display the full string

   ![get full connection string](./images/image-043.png)

4. Use the ***Copy button*** to copy and ***Paste the URI to your to your Param file.***

   ![copy connection string](./images/image-044.png)

### Insert the variables in the notebook

1. Navigate to the ***Data Engineering*** landing page using the experience picker

   ![navigate to Data Engineering](./images/image-045.png)

2. Navigate back to the ***QueenOfTheSkies_Notebook*** you uploaded earlier

   ![navigate to Notebook](./images/image-046.png)

3. In the cell at the top you will find the ***MyConnectionString*** variable that should be populated with the Custom app endpoint you created ealier and saved to your param file.

   ![insert connection string](./images/image-047.png)

4. Next is the ***SampleCsv*** variable that you should populate with the Sample file ABFS path that you captured earlier.

   ![insert abfs path](./images/image-048.png)

### Run the notebook and confirm the data is streaming

We will now be ready to run the notebook and stream the data to event stream. You may remember that we created a source but no destination. The reason is simple, we have no data in the pipe yet and the KQL db destination of event stream has to see some of it to allow its creation. So we will stream it now but let's remember that this will be an incomplete stream and we'll have to do it all again later.

1. Click ***Run all*** at the top.

   ![click on run all](./images/image-049.png)

2. Allow for the spark session to spin up and watch for logs at the bottom that will indicate that the stream has started. you should be seeing messages like this:

```code
*This was batch #:136
We loaded rows from: 1620 to row: 1632
Rows remaining in the stream: 134*
```

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

[^ Table of content ^](#table-of-content)

## Build the Power BI report

1. Download the Power BI template document (.PBIT) from the repo here and ***Save it to your local drive***
1. Open the PBIT file and wait for it to prompt you for the parameters
1. It should prompt you for two parameters
   1. ***MyQueryUri*** is the URI of your KQL db that you saved in your param file. 
   1. ***MyDatabaseName*** is the name you chose originally for the database.
1. Click ***Load*** and allow the report to connect and load data
1. It may require you to sign in to authenticate if not done already
1. ***Publish the report*** to your workspace

[^ Table of content ^](#table-of-content)
## Run the demo
1. Navigate back to the ***QoS_KQS*** KQL Query Set and to the ***QoS_DemoControl*** tab
1. Run the Clear table command to clean up the landing table
1. Run the ***Pre flight query** (see below) 
1. The result should be a *All set! Clear for takeoff* message

### Start the notebook
1. Navigate back to the ***QueenOfTheSkies_Notebook***
1. Click ***Run all*** 
1. Wait for the Logs at the bottom to show data streaming

### Run the report
1. Navigate to the ***QueenOfTheSky_rpt*** power BI report
1. To set the tone, read the text from the landing page
1. Then navigate to the second page to see the flight path. Note that until you start refreshing the path will not change. Don't wait too long or else the punchline will be spoiled

[^ Table of content ^](#table-of-content)

That's it folks! Enjoy running the demo and spread the word!