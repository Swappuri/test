// Student Names: Swapnil, Evan
// Student IDs: 001314459

create_database.sql code:
  create database nyc_taxi_database;
  use nyc_taxi_database;
  create table nyc_taxi_table(medallion char(32),
  hack_license char(32), 
  pickup_datetime datetime, 
  dropoff_datetime datetime, 
  trip_time_in_secs int(255), 
  trip_distance float(255, 2), 
  pickup_longitude decimal(9, 6),
  pickup_latitude decimal(8, 6),
  dropoff_longitude decimal(9, 6),
  dropoff_latitude decimal(8, 6),
  payment_type char(3),
  fare_amount float(255, 2),
  surcharge float(255, 2),
  mta_tax float(255, 2),
  tip_amount float(255, 2),
  tolls_amount float(255, 2),
  total_amount float(255, 2));

insert_data.py code:
  import csv
  import mysql.connector
  
  mydb = mysql.connector.connect(
    host = "localhost",
    user = "root",
    password = "",
    database = "nyctaxidatabase"
  )
  
  mycursor = mydb.cursor()
  
  sql = """insert into nyc_taxi_table (medallion, hack_license, pickup_datetime, dropoff_datetime, trip_time_in_secs,
                                  trip_distance, pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
                                  payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount)
                                  values (%s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s)"""
  
  batch_size = 10000
  rows = []
  
  with open("taxi-data-sorted-small.csv", "r") as f:
      reader = csv.reader(f)
      for row in reader:
          if len(row) == 17:
              rows.append(row)
              if len(rows) >= batch_size:
                  try:
                      mycursor.executemany(sql, rows)
                  except Exception as exception:
                      pass
                  rows = []
      if rows:
          try:
              mycursor.executemany(sql, rows)
          except Exception as exception:
              pass
  
  mydb.commit()
  mycursor.close()
  mydb.close()

clean_and_query.sql code:
  set SQL_SAFE_UPDATES = 0;
  
  delete from nyc_taxi_table
  where total_amount = 0 or trip_distance = 0;
  
  select avg(total_amount) as average_taxi_cost
  from nyc_taxi_table;
  
  select max(total_amount) as max_total_amount, min(total_amount) as min_total_amount
  from nyc_taxi_table;
  
  select hack_license, count(*) as ride_count
  from nyc_taxi_table
  group by hack_license
  order by ride_count desc
  limit 1;
  
  select payment_type, avg(tip_amount) as average_tip
  from nyc_taxi_table
  group by payment_type;

// ## Course Number: 3337
// ## Course Name: Database Theory and Applications
// ## Date Created: 02/02/2025
