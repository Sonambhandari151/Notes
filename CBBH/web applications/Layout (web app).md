Mainly divided intro 3 categories.


| Category                         | Description                                                                                                                                                                                                                                                          |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Web Application Infrastructure` | Describes the structure of required components, such as the database, needed for the web application to function as intended. Since the web application can be set up to run on a separate server, it is essential to know which database server it needs to access. |
| `Web Application Components`     | The components that make up a web application represent all the components that the web application interacts with. These are divided into the following three areas: `UI/UX`, `Client`, and `Server` components.                                                    |
| `Web Application Architecture`   | Architecture comprises all the relationships between the various web application components.                                                                                                                                                                         |

---
## Web application Infrastructure
Web applications can use many different infrastructure setups. These are also called `models`. The most common ones can be grouped into the following four types:

- `Client-Server`
- `One Server`
- `Many Servers - One Database`
- `Many Servers - Many Databases`

### 1. Client-server
![[Pasted image 20250703171935.png]]
In this model, Web applications consist of two parts:

- **Front-end**: runs in the browser (client-side),
    
- **Back-end**: runs on the server (server-side).
    

When a user accesses the app (e.g., `https://www.acme.local`) and interacts with it (like clicking a button), the browser sends an **HTTP request** to the server. The server processes it (e.g., login, cart update), then responds with data, which the browser displays in a user-friendly way.

---
## 2. One server
![[Pasted image 20250703174330.png]]
This architecture hosts the **entire web application and database on a single server**, making it easy to set up but highly risky.

If **one web app is compromised**, **all data and apps on the server are at risk** — a true _"all eggs in one basket"_ scenario.  
Also, if the **server goes down**, all apps become **unavailable** until it's fixed.

---
## 3. Many servers- One Database
![[Pasted image 20250703174824.png]]


- This model **separates the database** onto its **own database server**.
    
- The **web application hosting server** accesses the database server to **store and retrieve data**.
    
- It can be either **many servers to one database** or **one server to one database**,  
    as long as the **database is hosted separately** on its own server.
    

### 📊 Key Points:

- Several **web applications** can access a **single database**,  
    allowing **shared access to data** without needing to sync it.
    
- These web applications can be:
    
    - **Replications** of a main application (i.e., **primary/backup**), or
        
    - **Separate applications** that share **common data**.
        

### 🔐 Security Advantage – Segmentation:

- Each main component of the web application is **hosted separately**.
    
- If **one web server is compromised**, the **others are not directly affected**.
    
- If the **database is compromised** (e.g., by SQL injection), the **web application itself is still safe**.
    

### ⚙️ Additional Measures:

- Even after **asset segmentation**, you must implement **access controls**.
    
- Web applications should be allowed to access **only the data needed** to function as intended.
    

---
## 4. Many server - Many databases
![[Pasted image 20250703175616.png]]

- This model builds on the **Many Servers – One Database** setup.
    
- Inside the **database server**, each **web application’s data** is stored in a **separate database**.
    
- Each web app can access:
    
    - Its own **private data**, and
        
    - Only **common shared data**.
        
- It's also possible to host **each database** on a **separate database server**.
    
### 📊 Key Points:

- Used for **redundancy**:
    
    - If one **web server or database** goes offline, a **backup runs** in its place.
        
    - Helps **reduce downtime**.
        
- **More complex** to implement.
    
    - May require **load balancers** and proper setup.
        

### 🔐 Security Advantages:

- Strong in terms of **access control**.
    
- Good **asset segmentation** (each app and DB is separate).
    
- One of the **best architecture choices** for **security** and **availability**.
    

---
Aside from these models, there are other web application models available such as [serverless](https://aws.amazon.com/lambda/serverless-architectures-learn-more) web applications or web applications that utilize [microservices](https://aws.amazon.com/microservices).

---
## Web Application Components

Each web application can have a different number of components. Nevertheless, all of the components of the models mentioned previously can be broken down to:

1. `Client`
2. `Server`
    - Webserver
    - Web Application Logic
    - Database
3. `Services` (Microservices)
    - 3rd Party Integrations
    - Web Application Integrations
4. `Functions` (Serverless)

---
## Web Application Architecture
Three tier architecture.

| Layer              | Description                                                                                                                                                                                                         |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Presentation Layer | Consists of UI process components that enable communication with the application and the system. These can be accessed by the client via the web browser and are returned in the form of HTML, JavaScript, and CSS. |
| Application Layer  | This layer ensures that all client requests (web requests) are correctly processed. Various criteria are checked, such as authorization, privileges, and data passed on to the client.                              |
| Data Layer         | The data layer works closely with the application layer to determine exactly where the required data is stored and can be accessed.                                                                                 |
![[Pasted image 20250703180815.png]]

## Microservices

* Microservices are **independent components** of a web application.
* Usually built to perform **one specific task**.
### 🛒 Example in an Online Store:

* **Registration**
* **Search**
* **Payments**
* **Ratings**
* **Reviews**

### 🔗 Communication:
==Stateless= no memory of past interactions==
* Microservices **communicate with each other and the client**.
* Communication is **stateless** → request and response are independent.
* Data is **stored separately** from the microservice itself.
### 🧱 Architecture:

* Use of microservices is considered **Service-Oriented Architecture (SOA)**.
* Each microservice is a **small automated function** with a **specific business goal**.
* Microservices are **dependent** on each other to work as a system.
### 💻 Programming:

* Microservices can be written in **different programming languages**.
* They can still **interact and work together**.

### ✅ Benefits of Microservices:

* **Agility**
* **Flexible scaling**
* **Easy deployment**
* **Reusable code**
* **Resilience**
AWS [whitepaper](https://d1.awsstatic.com/whitepapers/microservices-on-aws.pdf)( provides overview of microservice implementation).
---
## serverless

- **Cloud providers** like AWS, GCP, Azure offer **serverless architectures**.
    
- These platforms provide **application frameworks** to build web apps **without managing servers**.

### ⚙️ How it works:

- Web apps run in **stateless computing containers** (e.g., **Docker**)
    
- The **cloud provider** handles:
    
    - **Server provisioning**
        
    - **Scaling**
        
    - **Maintenance**
        
### ✅ Benefits:

- No need to manage **infrastructure**
    
- Companies get **flexibility** to build and deploy apps/services easily
    
- **Infrastructure tasks are automated** by the provider
    
about serverless computing : https://aws.amazon.com/serverless/

---
## Architecture Security
* Understanding the **general architecture** and the **specific design** of a web app is crucial for penetration testing.
* Vulnerabilities may come from **design flaws**, not just **coding errors**.
### ⚠️ Example 1: Poor Access Control (Design Flaw)

* App functionality is securely implemented
* But without **proper Role-Based Access Control (RBAC)**:

  * Users might access **admin features** unintentionally
  * Or access **private data** of other users
* Fixing this requires a **major redesign** — **costly and time-consuming**

### ⚠️ Example 2: Segmented Database

* After gaining access to the **back-end server**, if the **database isn’t found**, it may be:

  * **Hosted separately**
  * **Split across multiple servers**
* You may only see **part of the data**

* **Security must be considered at every phase** of web app development
* **Penetration tests** should be done throughout the **development lifecycle**

---

