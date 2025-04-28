# Secure_Check_Police_Post_Log #

***LIBRARIES***
import streamlit as st - streamlit used for build web app
import pandas as pd - pandas hepls for handling the data
import pymysql  - it purpose to connect with python to mysql database
import plotly.express as px - it show for creating chart

path="traffic_stops - traffic_stops_with_vehicle_number (1).csv"
df=pd.read_csv(path) - this code for converts to raw file to structured tabel

df.isnull().sum() - this code detect the missing data or value

df.dropna(subset=['search_type'],inplace=True)-this functiion is used to remove specific rows

***THIS CODE TO CONNECTS TO MYSQL DATABASES***
import mysql.connector
mydb=mysql.connector.connect(
    host="localhost",
    user="root",
    password=""
)
mycursor=mydb.cursor()
print("your SQL is connected")


mycursor.execute("CREATE DATABASE police_log") - this code to create the database for mysql


mycursor.execute("USE police_log")
mycursor.execute("""
CREATE TABLE police_log(
    stop_date DATE,
    stop_time TIME,
    country_name VARCHAR(100),
    driver_gender VARCHAR(10),
    driver_age_raw VARCHAR(20),
    driver_age INT(2),
    driver_race VARCHAR(50),
    violation_raw TEXT,
    violation VARCHAR(100),
    search_conducted BOOLEAN,
    search_type VARCHAR(100),
    stop_outcome VARCHAR(100),
    is_arrested BOOLEAN,
    stop_duration VARCHAR(50),
    drugs_related_stop BOOLEAN,
    vehicle_number VARCHAR(50)
)
""")
mydb.commit() - this code for create table for database

from tabulate import tabulate - this library to represent the data into readable table format

mycursor.execute("SELECT * FROM police_log")
out=mycursor.fetchall()
print(tabulate(out,headers=[i[0] for i in mycursor.description],tablefmt='psql')) - this code main purpose to data show the tabulate format

# MEDIUM LEVEL QUERY #

("SELECT COUNT(*) AS total_police_stops FROM police_log") - this code execute to count the total number of police stops

("SELECT violation,COUNT(*) AS stops_count FROM police_log GROUP BY violation")- this code execute to count each violation 

("SELECT stop_outcome,COUNT(*) AS outcome_count FROM police_log WHERE stop_outcome IN('Arrest','Warning') GROUP BY stop_outcome")- this code execute for count the arrest or warning 

("SELECT AVG(driver_age) AS avg_driver_age FROM police_log WHERE driver_age IS NOT NULL") - this code execute for calculate to driver_age  average














