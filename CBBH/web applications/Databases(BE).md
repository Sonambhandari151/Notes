
- Web apps use **back-end databases** to store:
    
    - Core assets (e.g., images, files)
        
    - Application content (e.g., posts, updates)
        
    - User data (e.g., usernames, passwords)
        
- Enables **dynamic content** and **data retrieval** per user.
    

### 🔑 Key Characteristics Developers Look For:

- **Speed** in data storage and retrieval
    
- **Scalability** for growing apps
    
- **Capacity** for large datasets
    
- **Cost-effectiveness**
    

---

## 🟩 Relational Databases (SQL)

### 📌 Structure

- Stores data in **tables (rows & columns)**
    
- Uses **keys** to relate tables → forms a **schema**
    

### 🧱 Example Schema:
- The relationship between tables within a database is called a Schema.


- `users` table: `id`, `username`, `first_name`, `last_name`
    
- `posts` table: `id`, `user_id`, `date`, `content`
    
- Link `users.id` ↔ `posts.user_id`
    

### 🔁 Advantages:

- **Fast and reliable** with structured data
    
- Easy **cross-table queries** using JOINs
    
- Ensures **data integrity and normalization**
    
![[Pasted image 20250705084303.png]]
### 📚 Popular Relational Databases:

| Type           | Description                                                                     |
| -------------- | ------------------------------------------------------------------------------- |
| **MySQL**      | Most used, open-source, free                                                    |
| **MSSQL**      | Microsoft’s SQL server, integrates with IIS                                     |
| **Oracle**     | A very reliable database for big businesses,Enterprise-grade, robust but costly |
| **PostgreSQL** | Open-source, highly extensible                                                  |

#### 🔸 Others:

- SQLite, MariaDB, Amazon Aurora, Azure SQL
    

---

## 🟥 Non-Relational Databases (NoSQL)

### 📌 Overview

- **Schema-less**, no tables/columns/rows
    
- Best for **flexible, unstructured** or evolving data
    
- Highly **scalable and adaptable**
    

### 📦 4 Common Storage Models:

1. **Key-Value**: Pairs (e.g., JSON or XML)
    
2. **Document-Based**: Rich, structured documents (JSON)
    
3. **Wide-Column**: Column-family based storage (e.g., Cassandra)
    
4. **Graph**: Nodes and relationships (e.g., Neo4j)
    
![[Pasted image 20250705084354.png]]
### 🧾 Example: Key-Value in JSON

```json
{
  "100001": {
    "date": "01-01-2021",
    "content": "Welcome to this web application."
  },
  "100002": {
    "date": "02-01-2021",
    "content": "This is the first post on this web app."
  }
}
```
- The `Document-Based` model stores data in complex `JSON` objects and each object has certain meta-data while storing the rest of the data similarly to the `Key-Value` model.
### 📚 Popular NoSQL Databases:

| Type              | Description                                   |
| ----------------- | --------------------------------------------- |
| **MongoDB**       | Document-based, stores JSON, free/open-source |
| **ElasticSearch** | Optimized for fast data search and analysis   |
| **Cassandra**     | Scalable, fault-tolerant wide-column store    |

#### 🔸 Others:

- Redis, Neo4j, CouchDB, Amazon DynamoDB
    

---

## 🖥️ Database Use in Web Applications

### 📋 Setup Example (PHP + MySQL)

```php
// Connect to server
$conn = new mysqli("localhost", "user", "pass");

// Create a new database
$sql = "CREATE DATABASE database1";
$conn->query($sql);
```

### 📂 Querying the Database

```php
// Connect to database
$conn = new mysqli("localhost", "user", "pass", "database1");

// Query table
$query = "SELECT * FROM table_1";
$result = $conn->query($query);
```

### 🔍 Searching with User Input

```php
$searchInput = $_POST['findUser'];
$query = "SELECT * FROM users WHERE name LIKE '%$searchInput%'";
$result = $conn->query($query);

// Display result
while($row = $result->fetch_assoc()){
  echo $row["name"]."<br>";
}
```

---

## ⚠️ Security Note: SQL Injection

- Using raw input in queries (as above) is **vulnerable**.
    
- Use **prepared statements** or **parameterized queries** to avoid SQL injection.
    

---
