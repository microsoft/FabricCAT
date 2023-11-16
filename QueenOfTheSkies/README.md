# Queen of the skies demo
This demo will allow you to leverage the elements of Fabric Real-Time Analytics to showcase a powerful demo with a compeling story.

## Demo backstory
On February 1st, 2023, something very special happened in Everett, WA. Atlas Air, a well-known cargo plane operator, took delivery of a new airplane. Only this plane happens to be the last of the Boeing 747 that will ever be made.

## Table of content

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
   - Fill in the required parameters for the template
- Run the demo
   - Prepare the landing table
   - Run the preflight check
   - Start the notebook
   - Run the report

## Create a KQL database and KQL Queryset

1. Ensure you asre in the real time analytics experience by clicking the experience picker in the lower left corner and selecting Real-Time Analytics
1. Click the "KQL database" button at the top of the screen
1. Name your database: ***QueenOfTheSkies*** 
1. Navigate back to ***My workspace*** using the shortcut to the left of the screen
1. Click the **New** dropdown menu to the top left of the screen and select ***KQL Query set*** 
1. Name your query set: ***Qos_KQS***

### Structure your KQL Query set
1. Inside your KQL Queryset, create **two additional tabs** using the ***+*** sign next to the first tab.
   1. Name your first tab ***QoS DDL***
   1. Name your second tab ***Generate Planned flight***
   1. Name your third tab ***QosDemo control***
1. From this repo copy and paste the KQL script found in the three files to their corresponding tabs in the KQL Queryset

### Run the database preparation script

1. Run the code contained in the ***QoS DDL*** script
1. Ensure all operation are successful

### Ingest the sample file
We now will ingest the full flight path all at once. We do this so we can use the complete dataset to reconstruct the planned flight by surgically removing the section of the flight path we do not want and inferring the *planned* data points. 

1. Navigate back to the ***QueenOfTheSKies*** KQL database.
1. Click on the down chevron next to the ***Get Data*** button and select ***Local file***
1. 

### Rehydrate the planned flight table

### Prepare for the first run

## Create a Lakehouse

### Upload the sample file

## Upload the notebook

### Run the notebook and confirm the data is streaming

## Create the event stream

### Create a custom app source

### Create a KQL database destination

#### Complete the data mapping

## Build the Power BI report

### Generate the report using the Power BI report template

### Fill in the required parameters for the template

## Run the demo

### Prepare the landing table

### Run the preflight check

### Start the notebook

### Run the report