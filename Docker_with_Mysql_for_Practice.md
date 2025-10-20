```bash
docker pull mysql
docker images

```

<img width="1680" height="1050" alt="Screenshot 2025-10-20 at 6 46 56 PM" src="https://github.com/user-attachments/assets/1293c3d0-f0c7-4bef-8ae3-d6c530a555b5" />

<img width="1680" height="1050" alt="Screenshot 2025-10-20 at 6 47 25 PM" src="https://github.com/user-attachments/assets/5915dd7a-d0b9-4b05-8a88-2acf29e9aacb" />

## After Successfully pulling the image we need to assign the User ID and Password to mysql container 
```bash
docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=mydb -e MYSQL_USER=admin -e MYSQL_PASSWORD=admin -p 3306:3306 mysql
```

## Login into Mysql Container 
```bash
docker exec -it <IMAGE NAME> bash
```

## 🧑‍💻 Login into MySQL
```bash
docker exec -it mysql mysql -u admin -p   # Enter admin pass 
```


<img width="1680" height="1050" alt="Screenshot 2025-10-20 at 7 08 04 PM" src="https://github.com/user-attachments/assets/c2941b95-7df3-44a3-9ff5-434119eee79a" />

```bash
mysql> SHOW DATABASES ;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mydb               |
| performance_schema |
+--------------------+
3 rows in set (0.178 sec)

mysql> use mydb ;


CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(50),
  email VARCHAR(100),
  city VARCHAR(50)
);


mysql> SHOW TABLES;
+----------------+
| Tables_in_mydb |
+----------------+
| users          |
+----------------+
1 row in set (0.004 sec)


INSERT INTO users (name, email, city)
VALUES 
('Saime Shaikh', 'saime@example.com', 'Pune'),
('Sahil Khan', 'sahil@example.com', 'Mumbai'),
('Ayesha Khan', 'ayesha@example.com', 'Delhi'),
('Rahul Patil', 'rahul@example.com', 'Nagpur');


mysql> SELECT * FROM users;

- OutPut
+----+----------------+---------------------+--------+
| id | name           | email               | city   |
+----+----------------+---------------------+--------+
|  1 | Saime Shaikh   | saime@example.com   | Pune   |
|  2 | Sahil Khan     | sahil@example.com   | Mumbai |
|  3 | Ayesha Khan    | ayesha@example.com  | Delhi  |
|  4 | Rahul Patil    | rahul@example.com   | Nagpur |
+----+----------------+---------------------+--------+
4 rows in set (0.000 sec)


