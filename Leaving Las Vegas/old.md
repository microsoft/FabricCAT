# Fabric Real-Time Analytics - Leaving Las Vegas workshop

## Backstory
The OpenSky is a worldwide group of volunteer plane watchers using ADS-B receivers to track the transponder signals emitted by the civil aviation traffic in range of their receiver. The sum total of all the receivers build a worldwide history of aircraft tracking. The OpenSky Network allows the use of daily extracts of this data for research and learning purposes. Later in this workhop, you'll download this sample data. Please take great care to read the terms of use and licence.txt files. Abiding by these rule is what allows rich datasets like these to continue being available to us to use with training and learning.

In the interest of time, we're going to be using a special subset of this massive dataset so we can ensure a smooth experience. 

## Table of content
- [Environement setup](#lab-1-environment-setup)
- [Preparing the database](#lab-2-preparing-the-database)
- [Ingeting the OpenSky dataset](#lab-3-ingesting-opensky-dataset)
- [Exploring the dataset using KQL](#lab-4-exploring-the-dataset-using-kql)
- [Finding the streaming dataset](#lab-5-finding-the-streaming-datasets)
- [Building the streaming simulator](#lab-6-building-the-stream-simulator)
- [Deploy the visualization](#lab-7-deploy-the-visualizations)
- [Build a data activator alert](#lab-8-build-a-data-activator-alert)

## Lab 1: Environment setup

### Parameter File
Throughout this workshop, we will create manu services that need to talk to each other. To that end we will need to capture endpoints url, file paths and many more parameters. We suggest that at this point you start using a parameter file to capture the information at the moment of creation so you don't have to hunt down these parameters later down the line.

### Creating the OpenSky Lakehouse

1. In the Fabric experience picker at the bottom left of the screen, select ***Data Engineering***

   ![pick data engineering](./images/image-026.png)

2. In the section ***New*** click on the button ***LakeHouse***

   ![select new lakehouse](./images/image-027.png)

3. Name your lakehouse ***OpenSky_LH***

### Upload the files to one lake

1. In the Lakehouse menu, ***Right click*** on ***Files*** and then select ***Upload*** and finally ***Upload files***

   ![upload files](./images/image-029.png)

2. Use the folder button to navigate to where you saved the two data files located in the **Data** folder of this repo on your local machine and hit ***Open***

3. Click ***Upload***

### Save the ABFS path for the sample file

1. ***Right click*** on the  ***each of the file*** file in the lakehouse and hit ***Properties***

   ![click properties](./images/image-032.png)

2. Use the copy button next to the ***ABFS filepath***

   ![copy abfs path](./images/image-033.png)

3. **Paste the ABFS paths for both data files to your to your Param file.**


### Creating the OpenSKy KQL Database

1. Open the [Microsoft Fabric Webpage](https://app.fabric.microsoft.com/home)
2. Ensure you are in the real time analytics experience by clicking the experience picker in the lower left corner and selecting Real-Time Analytics

   ![Real time analytics experience](./images/image-001.png)

3. Click the "KQL database" button at the top of the screen

   ![KQL Database button](./images/image-002.png)

4. Name your database: ***OpenSky_KDB*** 

   ![name database](./images/image-003.png)

5. On the main page of the KQL Database, note the Query URI with a dedicated button to copy the URI. Click this button and **Save the Query URI AND the Database name to your Param file.**

   ![copy URI](./images/image-004.png)

6. Navigate back to ***My workspace*** using the shortcut to the left of the screen

   ![back to my workspace](./images/image-005.png)

7. Click the **New** dropdown menu to the top left of the screen and select ***KQL Query set*** 

   ![Create KQL Query Set](./images/image-006.png)

8. Name your query set: ***OpenSky_KQS*** and click on **Create**.

   ![Name query set](./images/image-007.png)

9. Select the ***OpenSky_KDB*** Database and click on **Connect**

   ![Select database](./images/image-008.png)

### Structure your KQL Query set

1. Inside your KQL Queryset, create **two additional tabs** using the ***+*** sign next to the first tab.
   1. Name your first tab ***OpenSky DDL***
   2. Name your second tab ***OpenSky Exploration***
   3. Name your third tab ***OpenSky Stream***
   4. Name your third tab ***OpenSky Demo Control***

      ![Insert KQL Query set](./images/image-009.png)

2. From this repo copy and paste the content of the KQL scripts found in [the repo](./Code/KQL/) to their corresponding tabs in the KQL Queryset

      ![copy content](./images/image-010.png)

[^Table of content^](#table-of-content)


### Creating the OpenSKy KQL Query set

### Creating the OpenSky Event Stream



## Lab 3: Ingesting OpenSky dataset

For best results, you should ingest the 2023-06-27 daily file in order to follow along with the exercises below. Feel free to download and ingest more in the future and keep exploring!

The data for this can be found here: [OpenSky daily state vector]()

The files are CSVs compressed in Gzip and packged as tarballs, you will need to manually download all 24 files to get the full day. 

Use the KQL Database "Get Data" experience and ingest only one 

[^Table of content^](#table-of-content)

## Lab 4: Exploring the dataset using KQL

The OpenSky exploration tab should have all the exploration queries.

TODO: Describes the queries

[^Table of content^](#table-of-content)

## Lab 5: Finding the streaming datasets

Now comes the fun part, we want to create a streaming dataset that will generate a meaningful (and date we say impressive) effect when visualized. To achieve this we will do these things in order:

1. Choose a single airport somewhere in the world. For this workshop we can use Las Vegas
1. Then we will single out one specific airline. We do this to artificially limit the number of records so the visual effect is cleaner. You are welcome to tweak these through your own experimentation.
1. Now we can answer this question: Show me all the flights that departed from the airport we chose. But wait, if we do this we have a timing problem, not all planes left at the same time... we could speed up time (but that would be rather complicated given our current understanding of physics). Kusto is a time series database, so it should instead be rather easy to timeshift all the timestamps of all the data points so they share a common departure time.
1. The above will thus create a nice bloom effect where all planes leave all at once in different directions.

[^Table of content^](#table-of-content)

## Lab 6: Building the Stream simulator

1. Download the [***OpenSky_nb.jpynb***](/Code/Notebooks/QueenOfTheSkies_Notebook.ipynb) file from this repo and save it to your local machine

2. Navigate back to the ***Data engineering landing page*** by using the experience picker at the bottom left corner of the screen.

   ![select local file](./images/image-034.png)

3. Click the ***Import Notebook*** button.

   ![select local file](./images/image-035.png)

4. and click the ***Upload*** button

   ![select local file](./images/image-036.png)

5. Navigate to your local file system where you saved the notebook file and hit ***Open***

   ![select local file](./images/image-037.png)

[^Table of content^](#table-of-content)

## Lab 7: Deploy the Visualizations

1. Download the Power BI template document (.PBIT) from the repo here and ***Save it to your local drive***. If you cloned the Git Hub Repository you will find the Power BI Report in folder `Code/PBI`.

2. Open the PBIT file and wait for it to prompt you for the parameters

3. It should prompt you for two parameters
   1. ***MyQueryUri*** is the URI of your KQL db that you saved in your param file.
   2. ***MyDatabaseName*** is the name you chose originally for the database.

14 Click ***Load*** and allow the report to connect and load data

   ![click on load](./images/image-061.png)

4.It may require you to sign in to authenticate if not done already

   ![authenticate](./images/image-062.png)

5.***Publish the report*** to your workspace

   ![publish the report](./images/image-063.png)

[^Table of content^](#table-of-content)

## Lab 8: Build a data activator alert

We're streaming telemetry from airplanes that are, for the most part, flying at crusing speed and staying relatively still. That's not very interessting to build an alert on. 

Frortunatly for us, these airplane are also moving in space and in time. We could create a trigger that will notify us whenever a plane's lattitude has become lower than 30. Since the dataset captures latitude in milidegrees, we should set the trigger condition to lat < 0.30. Why 30 you ask? For navigation afficionados here, latidude 30 is the tropic of the cancer, a very important line on the glove that all navigator use to guage how far they are on their journey. We're then going to set up an alert that will fire whenever a data point comes in with a latitude smaller than 30. But that could be a little misleading. We're going to add a condition that will check if the heading is between 160 and 200. Hading 180 is due South so we're essentially triggering on aircrafts that aew flying due south and are below the tropic of the cancer.

Let's build this as an additional source for the existing Event Stream.

[^Table of content^](#table-of-content)
