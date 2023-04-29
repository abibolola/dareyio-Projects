# IMPLEMENT A CLIENT SERVER ARCHITECTURE USING MYSQL DATABASE MANAGEMENT SYSTEM (DBMS).

- Create and configure two Linux-based virtual servers (EC2 instances in AWS).
```
Server A name - `mysql server`
Server B name - `mysql client`
```

- Install MySQL Server software on the MySQL server with the secure installation configuration

```sudo apt install mysql_server```

```sudo mysql_secure_installation```

- On mysql client Linux Server install MySQL Client software

```sudo apt install mysql_client```

- MySQL server uses TCP port 3306 by default, so you will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups.