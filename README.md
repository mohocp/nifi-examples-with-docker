# Convert CSV to JSON, and Move data from Postgres to MongoDB Using NIFI & Docker
this Repository contains Docker-compose for Nifi, Postgresql, pgAdmin and MongoDB. and NIFI templates for Convert CSV to JSON and  Move data from Postgres to MongoDB

## Prerequisites 
* Install Python 3.6 or above
* Install [Docker](https://www.docker.com/)
* Install [Docker Compose](https://docs.docker.com/compose/install/)

## Installation
Clone the Repository and navigate to docker-nifi directory<br>
Then run the docker-compose file <br><br>
`docker-compose up -d`

## Usage
In your browser open [localhsot:8080](http://localhsot:8080)
### Convert CSV to JSON
* Import `Assignment_One_Convert_CSV_to_Json-final.xml` Template
* Add data.csv file in data directory
* Right click on the NIFI canvas then start
* The JSON file will be generated on output directory

### Move data from Postgres to MongoDB
* Import `Assignment_Two_Move_data_from_Postgres_to_MongoDB.xml` Template
* Using PGAdmin create database and run the data.sql file in the data directory
* Right click on the NIFI canvas then start
* The Data will added automatically to MongoDB


The screenshots below 

**Updoad the Template** <br>
![Updoad the Template](https://i.ibb.co/31XTT28/Screen-Shot-2021-05-15-at-3-41-07-AM.png) <br>
**Add the Template** <br>
![Add the Template](https://i.ibb.co/vJH2XzH/Screen-Shot-2021-05-15-at-3-42-31-AM.png) <br>
![Updoad the Template](https://i.ibb.co/WkHKLHJ/Screen-Shot-2021-05-15-at-3-43-12-AM.png)


## Information
The description for each process in both workflows as the following
### Convert CSV to JSON
The following processes used to achive the JOB <br>
* **GetFile Process** used to get file from the data directory
* **SplitRecord** used to spearte each record to spearted FileFlow
  - **CSVReader** service used for parsing the incoming data.  
  - **CSVRecordSetWrite** service used for writing the result to FileFlow
*  **QueryRecord** allow to filter the set of FileFlows based on age > 40 condition 
* **ExtractText** used to store the content of the fields in Attributes
* **ReplaceText** used to replace the content of FileFlow with JSON using nifi expression language to get the value of attributes
* **MergeContent** used to merge all FileFlows items in one FileFlow 
* **UpdateAttribute** used to update the file with uuid + date of today + json
* **PutFile** used to load the final JSON file in output directory
* **LogAttribute** two processes was added; the first one used to handle the unhappy and unwanted relationships i.e. (failure, origin, unmatched) and the other one used to debug on the last setep

The screenshot below describe the entire workflow <br>
![Convert CSV to JSON NIFI Workflow](https://i.ibb.co/9HbvJQj/Screen-Shot-2021-05-15-at-3-36-15-AM.png)

### Move data from Postgres to MongoDB
The following processes used to achive the JOB <br>
* **ExecuteSQLRecord** used to to fetch the data from the data Postgres
* **SplitRecord** used to spearte each record to spearted FileFlow
  - **CSVReader** service used for parsing the incoming data.  
  - **CSVRecordSetWrite** service used for writing the result to FileFlow
*  **QueryRecord** allow to filter the set of FileFlows based on age > 40 condition 
* **ExtractText** used to store the content of the fields in Attributes
* **ReplaceText** used to replace the content of FileFlow with JSON using nifi expression language to get the value of attributes
* **PutMongo** used to load the JSON records to MongoDB
* **LogAttribute** two processed was added; the first one used to handle the unhappy and unwanted relationships i.e. (failure, origin, unmatched) and the other one used to debug on the last setep

The screenshot below describe the entire workflow <br>
![Move data from Postgres to MongoDB NIFI Workflow](https://i.ibb.co/3CtYtWF/Screen-Shot-2021-05-15-at-7-37-49-PM.png)