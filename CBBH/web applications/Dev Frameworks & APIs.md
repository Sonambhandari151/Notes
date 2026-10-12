Some of the most common web development frameworks include:

- [Laravel](https://laravel.com/) (`PHP`): usually used by startups and smaller companies, as it is powerful yet easy to develop for.
- [Express](https://expressjs.com/) (`Node.JS`): used by `PayPal`, `Yahoo`, `Uber`, `IBM`, and `MySpace`.
- [Django](https://www.djangoproject.com/) (`Python`): used by `Google`, `YouTube`, `Instagram`, `Mozilla`, and `Pinterest`.
- [Rails](https://rubyonrails.org) (`Ruby`): used by `GitHub`, `Hulu`, `Twitch`, `Airbnb`, and even `Twitter` in the past.
---
## APIs
## 🌐 APIs in Web Applications

### 📌 Overview

- **API** = _Application Programming Interface_
    
- Acts as a **bridge between front end and back end**
    
- Allows **data exchange** and **function execution** between components
    
- Commonly used in **back-end development** for modern web apps
    
### 🔁 How APIs Work in Web Applications

1. **Front end** makes a request to the **API**
    
2. API passes the request to the **back-end server**
    
3. Back end **processes** the request and performs a task
    
4. Server sends a **response** back via the API
    
5. **Front end renders** the output to the user
    

---

## 📤 HTTP Request Parameters

### 📍 Two Common Methods:

1. **GET parameters**
    
    - Passed in the URL
        
    - Example:  
        `/search.php?item=apples`
        
2. **POST parameters**
    
    - Sent in the **body** of the HTTP request
        
    - Example:
        
        ```
        POST /search.php HTTP/1.1
        Host: example.com
        ...
        item=apples
        ```
        

### ✅ Use Case

- Allows the front end to send **specific values** to a page
    
- Back end **processes input** and sends back **appropriate output**
    
- Enables **dynamic input handling** for a single page (e.g., search)
    

---

## 🌍 Web APIs

### 📌 Definition

- A **Web API** is an interface over the **HTTP protocol** that allows **remote access** to backend functions.
    
- Enables communication **between web applications** or services.
    

---

### 🔄 Examples

- **Weather API**:
    
    - Send: city name/id
        
    - Receive: weather data in JSON
        
- **Twitter API**:
    
    - Retrieve tweets
        
    - Post a tweet (with authentication)
        
    - Format: **JSON** or **XML**
        

---

### 🛠️ API Standards

- **REST** (Representational State Transfer)
    
- **SOAP** (Simple Object Access Protocol)
    
---
## 🧼 SOAP (Simple Object Access Protocol)


- **SOAP** is a protocol for exchanging structured data between front end and back end.
    
- Uses **XML** for both requests and responses.
    
- Typically works over **HTTP** (though it can work over other protocols too).
    

---

### SOAP Message Format (Example)

```xml
<?xml version="1.0"?>
<soap:Envelope
 xmlns:soap="http://www.example.com/soap/soap/"
 soap:encodingStyle="http://www.w3.org/soap/soap-encoding">
 
  <soap:Header>
  </soap:Header>
  
  <soap:Body>
    <soap:Fault>
    </soap:Fault>
  </soap:Body>

</soap:Envelope>
```

---

### ✅ Key Features

- Uses **XML** for structured data transmission.
    
- Can **transfer entire class objects** and even **binary data**.
    
- Useful for:
    
    - Sharing **complex or stateful objects**
        
    - **Serialized data exchange**
        
    - **Mobile and modern web apps** where component state is tracked or changed
        

---

### ⚠️ Limitations

- **Harder for beginners**
    
- **Verbose and complex syntax**
    
    - Even **simple queries** (like a search) can require long XML messages
        
- Slower compared to REST for lightweight operations
    

---

## REST

The `REST` ([Representational State Transfer](https://en.wikipedia.org/wiki/Representational_state_transfer)) standard shares data through the URL path 'i.e. `search/users/1`', and usually returns the output in `JSON` format 'i.e. userid `1`'.

Unlike Query Parameters, `REST` APIs usually focus on pages that expect one type of input passed directly through the URL path, without specifying its name or type. This is usually useful for queries like `search`, `sort`, or `filter`. This is why `REST` APIs usually break web application functionality into smaller APIs and utilize these smaller API requests to allow the web application to perform more advanced actions, making the web application more modular and scalable.

Responses to `REST` API requests are usually made in `JSON` format, and the front end components are then developed to handle this response and render it properly. Other output formats for `REST` include `XML`, `x-www-form-urlencoded`, or even raw data. As seen previously in the `database` section, the following is an example of a `JSON` response to the `GET /category/posts/` API request:

Code: json

```json
{
  "100001": {
    "date": "01-01-2021",
    "content": "Welcome to this web application."
  },
  "100002": {
    "date": "02-01-2021",
    "content": "This is the first post on this web app."
  },
  "100003": {
    "date": "02-01-2021",
    "content": "Reminder: Tomorrow is the ..."
  }
}
```

`REST` uses various HTTP methods to perform different actions on the web application:

- `GET` request to retrieve data
- `POST` request to create data (non-idempotent)
- `PUT` request to create or replace existing data (idempotent)
- `DELETE` request to remove data