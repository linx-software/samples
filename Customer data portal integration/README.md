# Customer data portal integration

## Overview 
A CSV file containing Customer related data is dropped into a file server directory. 
When the file is picked up, the Customer data is retrieved from the file and then inserted into a SQL database row by row. 
A Linx REST web service then returns the Customer data from the database when requested from a front-end portal (UI Bakery).

## Process Flow

### Automate data import from file to database (File/Database) - 1/3
A custom Linx process [ImportFileIntoDatabase] is created which reads a CSV file, line by line, and then inserts the fields of each row into a database table. 
For each customer, data manipulations are performed based on a number of conditions. 
Following this, the file is then moved to a ‘processed’ location. 

A Timer service is created which executes on a 1 min interval. 
The timer service performs a list of all the files within a directoy, when a file is picked up, the custom file processing process [ImportFileIntoDatabase]  is executed for each file.

### Expose Customer Data to front-end portal (REST API)  2/3
A custom Linx process [GetCustomersFromDatabase] is created which queries the database for the customer information that was imported from the CSV file and returns it. 
This process is then exposed as a REST operation. The web service is then requested to return the data from the database which is used to display data on a front-end screen (UI Bakery).

### Enrich Customer data with Geo-location – (HTTP Request)– 3/3
The REST operation is expanded to enrich the customer data with geo-location data using a HTTP Request. 
Using the stored postal code, for each customer, a HTTP Request is made to a REST web service which provides detailed information on area codes in the U.K. 
The longitude and latitude of each customers postal code are added to the response which is then displayed on the front-end (UI Bakery).


Postal code data provided by [Postcodes](https://postcodes.io/)

Front-end developed using [UI Bakery](https://uibakery.io/)
