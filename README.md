# 🔥 Apache Cassandra with AstraDatastax 🔥

> Developed for the Bachelor's Degree in Computer Science:
> - University: Universidade do Vale do Rio dos Sinos (Unisinos)
> - Subject: Database Management Systems (DBMS)
> - Semester: 2023/02
> - Students: Bel Cogo, Bruno Hoffmann, João Accorsi and Rafael Klauck

# Objective

Sample Implementation with Apache Cassandra Databases with AstraDatastax to demonstrate CRUD Operations.  
Cassandra is a NoSQL distributed database, and its documentation can be found in [Cassandra Basics](https://cassandra.apache.org/_/cassandra-basics.html).

## 1. Create your Astra DB instance
DataStax Astra DB simplifies cloud-native application development, and reduces time to install, deploy and scale.

✅ Register (if needed) and Sign In to [Astra DB](https://astra.datastax.com): You can use your `Github`, `Google` accounts or register with an `email`.  <br />
✅ Choose "Start Free Now" plan, then "Get Started" to work in the free tier.

## 2. Create your Cassandra Database

✅ In the Astra DB home page, choose Create Database.  <br />
✅ Fill the Database Name and Keyspace Name.  <br />
✅ For the region, you can choose any provider with the region closest to you.  <br />
✅ In Databases tab, you will see your databases in Pending statuses. In around 5 minutes it will be ready as Active.

https://github.com/JoaoAccorsi/Apache-Cassandra-Database/assets/60155867/94c17af7-3b96-43c0-8f31-1cf5b8adb47e

## 3. CQL Console, describe keyspaces and USE it

✅ 3a. CQL Console

Please navigate to the CQL console:

https://github.com/JoaoAccorsi/Apache-Cassandra-Database/assets/60155867/7453c9f3-7905-4a1d-811e-a9286c9bc48d

✅ 3b. Describe Keyspaces

Before creating a table, it is needed to tell the database which keyspace will be used. <br />
To do this, **_DESCRIBE_** (or "DESC") command show all of the keyspaces that are in the database. 

📘 **Command to execute**
```sql
DESC KEYSPACES;
```
📗 **Expected output**

![3b  Describe keyspaces](https://github.com/JoaoAccorsi/Apache-Cassandra-Database/assets/60155867/8b28e075-edd1-46ad-a441-fe03c9659a30)

We can see keyspace `tutorial` that we had initially created.

✅ 3c. USE the keyspace

Execute the USE command with the keyspace `tutorial` to tell the database the context with `tutorial`.

📘 **Command to execute**
```sql
USE tutorial;
```

📗 **Expected output**

![3c  USE the keyspace](https://github.com/JoaoAccorsi/Apache-Cassandra-Database/assets/60155867/4aab2f88-975c-4e6b-bd28-b0a8453f37e9)

The prompt displays ```<username>@cqlsh:tutorial>``` showing that the **_tutorial_** keyspace is being used. 

## 4. Create Tables

For this tutorial, a sample travel system was created, with tables `person`, `plane` and `trip`. <br />
To create the tables, consider the below CQL commands:

📘 **Command to execute**

```sql
CREATE TABLE IF NOT EXISTS person ( 
  person_id     UUID,
  name          TEXT,
  nationality   TEXT,	
  email         TEXT,
  PRIMARY KEY (person_id)
);

CREATE TABLE IF NOT EXISTS plane ( 
  plane_id    UUID,
  plane_name  TEXT, 
  year        TEXT,
  PRIMARY KEY (plane_id)
);

CREATE TABLE IF NOT EXISTS trip ( 
  trip_id         UUID, 
  person_id       UUID,
  plane_id        UUID,
  date            TEXT,     
  departure_city  TEXT,
  destinicy_city  TEXT,
  distance        TEXT,
  PRIMARY KEY ((trip_id), person_id, plane_id)
) WITH CLUSTERING ORDER BY (person_id DESC, plane_id DESC);
```

📗 **Expected output**

![4a  Create Tables](https://github.com/JoaoAccorsi/Apache-Cassandra-Database/assets/60155867/4ffe13c6-b462-4449-be4b-bc6a7e5ea9f2)

By the _DESC_ CQL command, you can see the created tables. <br /> <br />
📘 **Command to execute**
```sql
DESC TABLES;
```

📗 **Expected output**

![image](https://github.com/JoaoAccorsi/Apache-Cassandra-Database/assets/60155867/35b7328b-c503-4110-819d-c5d62442a1bb)

## 5. (C)RUD - Create
To populate the created tables with data, CQL command `INSERT` is used.

📘 **Command to execute**
```sql
// Insert into table person

INSERT INTO person (person_id, name, nationality, email) VALUES (
  11111111-1111-1111-1111-111111111111, 'William Griffin', 'Irish', 'william.griffin@gmail.com'
);

INSERT INTO person (person_id, name, nationality, email) VALUES (
  22222222-2222-2222-2222-222222222222, 'Jennifer Gooden', 'Australian', 'jennifer.gooden@gmail.com'
);

INSERT INTO person (person_id, name, nationality, email) VALUES (
  33333333-3333-3333-3333-333333333333, 'David Figman', 'Canadian', 'david.figman@gmail.com'
);

// Insert into table plane

INSERT INTO plane (plane_id, plane_name, year) VALUES (
  44444444-4444-4444-4444-444444444444, 'Airbus A380', '2020'
);

INSERT INTO plane (plane_id, plane_name, year) VALUES (
  55555555-5555-5555-5555-555555555555, 'Boeing 777', '2020'
);

INSERT INTO plane (plane_id, plane_name, year) VALUES (
  66666666-6666-6666-6666-666666666666, 'Airbus A320', '2023'
);

// Insert into table trip

INSERT INTO trip (trip_id, person_id, plane_id, date, departure_city, destinicy_city, distance) VALUES (
  77777777-7777-7777-7777-777777777777, 22222222-2222-2222-2222-222222222222, 
  55555555-5555-5555-5555-555555555555, '22/06/2023', 'Sao Paulo', 'New York', '7.681 km'
);

INSERT INTO trip (trip_id, person_id, plane_id, date, departure_city, destinicy_city, distance) VALUES (
  88888888-8888-8888-8888-888888888888, 33333333-3333-3333-3333-333333333333, 
  44444444-4444-4444-4444-444444444444, '02/02/2023', 'Sao Paulo', 'Madrid', '8.380 km' 
);

INSERT INTO trip (trip_id, person_id, plane_id, date, departure_city, destinicy_city, distance) VALUES (
  99999999-9999-9999-9999-999999999999, 11111111-1111-1111-1111-111111111111, 
  66666666-6666-6666-6666-666666666666, '10/04/2021', 'Porto Alegre', 'Curitiba', '751 km' 
);
```
📗 **Expected output**

![5  (C)RUD - Create](https://github.com/JoaoAccorsi/Apache-Cassandra-Database/assets/60155867/1ff43cbc-c030-4bfa-9c3f-6565deb21442)

## 6. C(R)UD - Read
To read the data stored in Cassandra , CQL command `SELECT` is used.

📘 **Command to execute**
```sql
SELECT * FROM person;

SELECT plane_id, plane_name, year FROM plane 
	WHERE year = '2020' ALLOW FILTERING;

SELECT trip_id, person_id, date, departure_city, destinicy_city, distance FROM trip
  WHERE departure_city = 'Sao Paulo' ALLOW FILTERING;

```
📗 **Expected output**

![6  C(R)UD - Read](https://github.com/JoaoAccorsi/Apache-Cassandra-Database/assets/60155867/28341976-4aa7-452e-861a-6b76e7374bd4)

## 7. CR(U)D - Update
To update the data stored in Cassandra , CQL command `UPDATE` is used.

📘 **Command to execute**
```sql
UPDATE person 
  SET email = 'william.griffin12345@gmail.com' 
    WHERE person_id = 11111111-1111-1111-1111-111111111111;
```
📗 **Expected output**

![7  CR(U)D - Update](https://github.com/JoaoAccorsi/Apache-Cassandra-Database/assets/60155867/3a547dd7-f4b9-48f4-aaba-c99d2e102fe6)

## 8. CRU(D) - Delete
To delete the data stored in Cassandra , CQL command `DELETE` is used.

📘 **Command to execute**
```sql
DELETE FROM plane
  WHERE plane_id = 55555555-5555-5555-5555-555555555555;
```
📗 **Expected output**

![8  CRU(D) - Delete](https://github.com/JoaoAccorsi/Apache-Cassandra-Database/assets/60155867/e9cea202-80fc-41a5-9f59-30ea9dd3af03)

## 9. Conclusion

With these results showed in this tutorial, we could see that Apache Cassandra is a flexible and powerful database. <br />
It´s use together with AstraDatastax is easy, and ideal for learning proposes.





