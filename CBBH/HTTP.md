- HTTP: application layer protocol used to access worldwide resources.
- **Hypertext:** Text containing links to other resources. 
- **Port:** 80
- We enter a **Fully Qualified Domain Name** as a **Uniform Resource Locator(URL)**.
- **FQDN:** eg: www.hackthebox.com
- **URL**![[Pasted image 20250626222803.png]]

| Component    | Description                                                                                                                                       |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| Scheme       | Used to identify the protocol .                                                                                                                   |
| User Info    | Contains credentials(optional)                                                                                                                    |
| Host         | SIgnifies resource location. (Hostname or IP)                                                                                                     |
| Port         | 80                                                                                                                                                |
| Path         | File or a folder(default : index.html)                                                                                                            |
| Query String | Starts with ?, consists parameter (e.g. **login**)and value(e.g. **true**). Multiple parameter separated by (**&**)                               |
| Fragments    | Fragments are processed by the browsers on the client-side to locate sections within the primary resource (e.g. a header or section on the page). |
![[Pasted image 20250626232035.png]]**Note:** Our browsers usually first look up records in the local '`/etc/hosts`' file, and if the requested domain does not exist within it, then they would contact other DNS servers. We can use the '`/etc/hosts`' to manually add records to for DNS resolution, by adding the IP followed by the domain name.

---
## Curl
- Web requests through Browser and cURL.
- **cURL** is command line tool and library that supports HTTP along with many other protocols. Used for scripting and automation.
- HTTP request: curl google.com ( prints raw format HTML/JS/CSS).
- To save to file : curl -O inlanefreight.com/index.html or mention filename.
- -s : to silent the status.
- --help all , or -h for help
- man curl for full manual page.
- -v or -vvv for verbose mode to get full HTTP request and response.


----
## HTTPS 
-  HTTPS is used coz it is encrypted unlike HTTP which sends clear text.
- **Note:** Although the data transferred through the HTTPS protocol may be encrypted, the request may still reveal the visited URL if it contacted a clear-text DNS server. For this reason, it is recommended to utilize encrypted DNS servers (e.g. 8.8.8.8 or 1.1.1.1), or utilize a VPN service to ensure all traffic is properly encrypted.
- ![[Pasted image 20250627000832.png]]
- If HTTP is requested, it redirects the request to HTTPS 443 via the **301 Moved Permanently.**
- Next, the client (web browser) sends a "client hello" packet, giving information about itself. After this, the server replies with "server hello", followed by a [key exchange](https://en.wikipedia.org/wiki/Key_exchange) to exchange SSL certificates. The client verifies the key/certificate and sends one of its own. After this, an encrypted [handshake](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake) is initiated to confirm whether the encryption and transfer are working correctly.

- Once the handshake completes successfully, normal HTTP communication is continued, which is encrypted after that.

- Attacker can downgrade HTTPS to HTTP using HTTP downgrade attack, making the data transferred in clear-text. This is done by setting **MITM**.
---
## cURL for HTTPS

- If website is contacted with invalid SSL certificate or outdated one, cURL by default would not proceed with communication.
- To skip the certificate check with cURL: 
    <pre>
    curl -k https://google.com
</pre>
---
## HTTP request
![[Pasted image 20250701110559.png]]**HTTP GET request**

| Field   | Example           | Description                                                   |
| ------- | ----------------- | ------------------------------------------------------------- |
| Method  | GET               | Specifies type of action to perform.                          |
| Path    | /users/login.html | Path to resource. Can also have query string(?username=user). |
| Version | HTTP/1.1          | HTTP version.                                                 |
**HTTP 1.X :** Sneds requests as clear-text. Uses new line character to separate different fields and requests.
**HTTP 2.X:** Sends requests as binary data in a dictionary fom.

---
## HTTP Response
![[Pasted image 20250701112110.png]]
- Contains 2 Fields.
- HTTP Version and HTTP response code.
- The response body is usually defined as HTML code. It can also respond with other code types such as JSON.

---
# HTTP Headers

## 1.General Headers
    - Used in both HTTP requests and responses.
    - Describe the message rather than its contents.

| Header         | Example           | Description                                                                                                         |
| -------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Date**       | Date: wed, 16...  | Time and Date of message originated.                                                                                |
| **Connection** | Connection: close | Dictates if connection should stay alive after the request finishes. It's value can be **close** or **keep alive.** |
## 2. Entity Headers
    - Can be common to both request and response.
    - Used to **describe the content(entity)** transferred by a message.
    - Usually found in responses and POST or PUT requests.


| Header             | Example                       | Description                                                                                                                                                                                                                              |
| ------------------ | ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Content- type      | Content-Type : text/html      | Describe the type of resources being transferred.The `charset` field denotes the encoding standard, such as [UTF-8](https://en.wikipedia.org/wiki/UTF-8)                                                                                 |
| `Media-Type`       | `Media-Type: application/pdf` | The `media-type` is similar to `Content-Type`, and describes the data being transferred. This header can play a crucial role in making the server interpret our input. The `charset` field may also be used with this header.            |
| Boundary           | `boundary="b4e4fbd93540"`     | Acts as a marker to separate content when there is more than one in the same message. For example, within a form data, this boundary gets used as `--b4e4fbd93540` to separate different parts of the form.                              |
| `Content-Length`   | `Content-Length: 385`         | Holds the size of the entity being passed. This header is necessary as the server uses it to read data from the message body, and is automatically generated by the browser and tools like cURL.                                         |
| `Content-Encoding` | `Content-Encoding: gzip`      | Data can undergo multiple transformations before being passed. For example, large amounts of data can be compressed to reduce the message size. The type of encoding being used should be specified using the `Content-Encoding` header. |

## 3. Request Headers
    - Client sends Request Headers in a HTTP transaction.
    - Used as an HTTP request and do not relate to the content of the message.


| Header          | Example                                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| --------------- | ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Host            | `Host: www.inlanefreight.com`            | Used to specify the host being queried for the resource. This can be a domain name or an IP address. HTTP servers can be configured to host different websites, which are revealed based on the hostname. This makes the host header an important enumeration target, as it can indicate the existence of other hosts on the target server.                                                                                                                          |
| User- Agent     | `User-Agent: curl/7.77.0`                | The `User-Agent` header is used to describe the client requesting resources. This header can reveal a lot about the client, such as the browser, its version, and the operating system.                                                                                                                                                                                                                                                                              |
| Referer         | `Referer: http://www.inlanefreight.com/` | Denotes where the current request is coming from. For example, clicking a link from Google search results would make `https://google.com` the referer. Trusting this header can be dangerous as it can be easily manipulated, leading to unintended consequences.                                                                                                                                                                                                    |
| Accept          | `Accept: */*`                            | The `Accept` header describes which media types the client can understand. It can contain multiple media types separated by commas. The `*/*` value signifies that all media types are accepted.                                                                                                                                                                                                                                                                     |
| Cookie          | `Cookie: PHPSESSID=b4e4fbd93540`         | Contains cookie-value pairs in the format `name=value`. A [cookie](https://en.wikipedia.org/wiki/HTTP_cookie) is a piece of data stored on the client-side and on the server, which acts as an identifier. These are passed to the server per request, thus maintaining the client's access. Cookies can also serve other purposes, such as saving user preferences or session tracking. There can be multiple cookies in a single header separated by a semi-colon. |
| `Authorization` | `Authorization: BASIC cGFzc3dvcmQK`      | Another method for the server to identify clients. After successful authentication, the server returns a token unique to the client. Unlike cookies, tokens are stored only on the client-side and retrieved by the server per request. There are multiple types of authentication types based on the webserver and application type used.                                                                                                                           |

## 4. Response Headers
    - can be used in an HTTP response and do not relate to the content.
    - response headers such as `Age`, `Location`, and `Server` are used to provide more context about the response.

| Header             | Example                                     | Description                                                                                                                                                                               |
| ------------------ | ------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Server             | `Server: Apache/2.2.14 (Win32)`             | Contains information about the HTTP server, which processed the request.                                                                                                                  |
| Set-Cookie         | `Set-Cookie: PHPSESSID=b4e4fbd93540`        | Contains the cookies needed for client identification. Browsers parse the cookies and store them for future requests. This header follows the same format as the `Cookie` request header. |
| `WWW-Authenticate` | `WWW-Authenticate: BASIC realm="localhost"` | Notifies the client about the type of authentication required to access the requested resource.                                                                                           |

## 5. Security Headers
      - a class of response headers used to specify certain rules and policies to be followed by the browser while accessing the website.



| Header                      | Example                                       | Description                                                                                                                                                                                                                                                                                                                                 |
| --------------------------- | --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Content-Security-Policy`   | `Content-Security-Policy: script-src 'self'`  | Dictates the website's policy towards externally injected resources. This could be JavaScript code as well as script resources. This header instructs the browser to accept resources only from certain trusted domains, hence preventing attacks such as [Cross-site scripting (XSS)](https://en.wikipedia.org/wiki/Cross-site_scripting). |
| `Strict-Transport-Security` | `Strict-Transport-Security: max-age=31536000` | Prevents the browser from accessing the website over the plaintext HTTP protocol, and forces all communication to be carried over the secure HTTPS protocol. This prevents attackers from sniffing web traffic and accessing protected information such as passwords or other sensitive data.                                               |
| `Referrer-Policy`           | `Referrer-Policy: origin`                     | Dictates whether the browser should include the value specified via the `Referer` header or not. It can help in avoiding disclosing sensitive URLs and information while browsing the website.                                                                                                                                              |
- `-I`: Sends a **HEAD request** and shows **only response headers**.
    
- `-i`: Sends a **normal request** (like GET) and shows **both headers and body**.
    
- Main difference: `-I` changes the request type to HEAD; `-i` does not.

## with cURL
- ### 🔹 `-H` flag
    Used to set **any** HTTP header manually.

- Instead of using `-H` for common headers, cURL provides specific options:
1. **`-A` for User-Agent**
    Sets the `User-Agent` header.

---
## HTTP Methods and Codes


### Request Methods

| **Method** | **Description**                                                                                                                                                                                                                                                                                                                      |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `GET`      | Requests a specific resource. Additional data can be passed to the server via query strings in the URL (e.g. `?param=value`).                                                                                                                                                                                                        |
| `POST`     | Sends data to the server. It can handle multiple types of input, such as text, PDFs, and other forms of binary data. This data is appended in the request body present after the headers. The POST method is commonly used when sending information (e.g. forms/logins) or uploading data to a website, such as images or documents. |
| `HEAD`     | Requests the headers that would be returned if a GET request was made to the server. It doesn't return the request body and is usually made to check the response length before downloading resources.                                                                                                                               |
| `PUT`      | Creates new resources on the server. Allowing this method without proper controls can lead to uploading malicious resources.                                                                                                                                                                                                         |
| `DELETE`   | Deletes an existing resource on the webserver. If not properly secured, can lead to Denial of Service (DoS) by deleting critical files on the web server.                                                                                                                                                                            |
| `OPTIONS`  | Returns information about the server, such as the methods accepted by it.                                                                                                                                                                                                                                                            |
| `PATCH`    | Applies partial modifications to the resource at the specified location.                                                                                                                                                                                                                                                             |
**Note:** Most modern web applications mainly rely on the `GET` and `POST` methods. However, any web application that utilizes REST APIs also rely on `PUT` and `DELETE`, which are used to update and delete data on the API endpoint, respectively.

---
## Response Codes
| **Type** | **Description**                                                                                                                  |
| -------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `1xx`    | Provides information and does not affect the processing of the request.                                                          |
| `2xx`    | Returned when a request succeeds.                                                                                                |
| `3xx`    | Returned when the server redirects the client.                                                                                   |
| `4xx`    | Signifies improper requests `from the client`. For example, requesting a resource that doesn't exist or requesting a bad format. |
| `5xx`    | Returned when there is some problem `with the HTTP server` itself.                                                               |
### Common codes

| **Code**                    | **Description**                                                                                                                                           |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `200 OK`                    | Returned on a successful request, and the response body usually contains the requested resource.                                                          |
| `302 Found`                 | Redirects the client to another URL. For example, redirecting the user to their dashboard after a successful login.                                       |
| `400 Bad Request`           | Returned on encountering malformed requests such as requests with missing line terminators.                                                               |
| `403 Forbidden`             | Signifies that the client doesn't have appropriate access to the resource. It can also be returned when the server detects malicious input from the user. |
| `404 Not Found`             | Returned when the client requests a resource that doesn't exist on the server.                                                                            |
| `500 Internal Server Error` | Returned when the server cannot process the request.                                                                                                      |
- Various servers and providers such as [Cloudflare](https://support.cloudflare.com/hc/en-us/articles/115003014432-HTTP-Status-Codes) or [AWS](https://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/APIError.html) implement their own codes.
- ---
## GET
- Default method called whenever we visit any URL to obtain the remote resources hosted at the URL.
   - ### **HTTP Basic Authenticationaaaaaaaa**
     HTTP Basic Auth is a simple authentication method where the **web server prompts for a username and password**. The browser sends credentials in the `Authorization` header using **Base64 encoding**. It's commonly used to protect specific pages or directories without using a login form.  
     Should be used with **HTTPS** to keep credentials secure.GET

```shell-session
s0nam@htb[/htb]$ curl -i http://<SERVER_IP>:<PORT>/
HTTP/1.1 401 Authorization Required
Date: Mon, 21 Feb 2022 13:11:46 GMT
Server: Apache/2.4.41 (Ubuntu)
Cache-Control: no-cache, must-revalidate, max-age=0
WWW-Authenticate: Basic realm="Access denied"
Content-Length: 13
Content-Type: text/html; charset=UTF-8

Access denied
```

- access denied in response body and basic realm = "access denied" in **WWW-Authenticate** header confirms that this page uses **basic HTTP auth**.
- ==TO provide credential through cURL , use -u flag.==


```
curl -u admin:admin http://<SERVER_IP>:<PORT>/
```

or 
```
curl http://admin:admin@<SERVER_IP>:<PORT>/
```

--- 
## HTTP Authorization
 By adding -v 
 ```
curl -v http://admin:admin@<SERVER_IP>:<PORT>/
```

- HTTP request sets the **Authorization : Basic YWRtaW46YWRtaW4=** (base 64 encoded of **admin:admin**) .
- If we use modern authentication method( eg JWT), Authorization: **Bearer** . It contains longer encrypted token.

#### Manually setting Authorization
```
curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/
```
---
## GET parameters
### 🔹 **Explanation: GET Parameters and Search Request Flow**

This section explains how **search functionality** on a web page works using **GET requests** and how you can inspect and replicate those requests using browser tools and `curl`.

---

### ✅ 1. **User Interface: City Search**

* You enter a city name in the search bar.
* When you hit **Enter**, the browser sends a **GET request** in the background to fetch matching cities.

---

### ✅ 2. **Viewing the Request in Browser DevTools**

* Open **DevTools → Network tab** (`Ctrl + Shift + E`).
* **Clear old requests** using the trash icon.
* Type a search term like `"le"` → Browser sends a request to:

  ```
  search.php?search=le
  ```
* The request is a **GET request** with the parameter `search=le`.

---

### ✅ 3. **Understanding the URL**

* The full URL looks like:

  ```
  http://127.0.0.1/search.php?search=le
  ```
* `search.php` is the backend file that handles the search.
* `search=le` is a **GET parameter** (sent in the URL).

---

### ✅ 4. **Replicating with curl**

You can copy the request and run it in the terminal:

```bash
curl 'http://<SERVER_IP>:<PORT>/search.php?search=le' \
-H 'Authorization: Basic YWRtaW46YWRtaW4='
```

* This sends the same GET request with **Basic Auth**.
* You’ll see the same output:

  ```
  Leeds (UK)
  Leicester (UK)
  ```

---

### ✅ 5. **Using Copy as Fetch**

* In DevTools > Network, right-click the request → **Copy > Copy as Fetch**

* Go to **Console** (`Ctrl + Shift + K`)

* Paste the fetch command:

  ```js
  fetch("http://127.0.0.1/search.php?search=le", {
    headers: {
      "Authorization": "Basic YWRtaW46YWRtaW4=",
    }
  }).then(res => res.text()).then(console.log)
  ```

* The request is sent, and the response is logged in the console.

---
SUMMARY

* Search input sends a **GET request** with query parameters.
* You can inspect and replicate it using **DevTools** or `curl`.
* `Copy as cURL` and `Copy as Fetch` are handy tools to debug or reuse requests.
* This helps understand how web apps fetch data dynamically without full page reloads.

---

## POST
- Passed within the HTTP Request body.
- Benefits:

1. **Lack of Logging**:  
    POST data is sent in the **body**, not the URL, so large uploads (e.g., files) aren’t stored in server logs like GET URLs would be.
    
2. **Less Encoding Needed**:  
    POST supports **binary data** in the body with minimal encoding, unlike GET URLs which require special characters to be encoded.
    
3. **More Data Can Be Sent**:  
    **GET URLs have length limits** (usually <2000 characters), but POST can handle **much larger amounts of data** in the body.

---
## Login Forms

- Web apps often use **HTML login forms** instead of HTTP Basic Auth.
    
- When logging in, the browser sends a **POST request** with credentials in the **body**:
    
    ```
    username=admin&password=admin
    ```
    
- You can inspect this request using **DevTools → Network tab**.
    
- To simulate the login using `curl`: In network request, copy as cURL.
- -X for POST, -d for credentials.
    
    ```bash
    curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/
    ```
    
- If the login redirects (e.g., to `/dashboard.php`), use `-L` with curl to follow it:
    
    ```bash
    curl -L -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/
    ```
    
- Seeing search UI in response confirms **successful authentication**.
    
---
## Authenticated Cookies
- Use the -v  or -i flag to view response, it contains Set-Cookie header with authwntication cookie.
 EG: Specify cookie as a header.
  ```
curl -H 'Cookie: PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/
```
or use cookie value with -b flag
```
curl -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/
```

### In browser
- In Dev tools -> **storage tab** (shift+ F9). if we were logged out, then our PHP cookie should not be authenticated.
- simply replace the cookie value with our own. Otherwise, we can right-click on the cookie and select `Delete All`, and the click on the `+` icon to add a new cookie.
- After that, we need to enter the cookie name, which is the part before the `=` (`PHPSESSID`), and then the cookie value, which is the part after the `=` (`c1nsa6op7vtk7kdis7bcnbadf1`).
- Refresh, we are logged in.  Used in XSS.
---
## JSON Data
- After searching through network devtools, we get follwing JSON Data.
    - ```json
  {"search":"london"}
- Conent-type will be : application/json.
- RIght click on request, copy request Headers. Content-type verified.
- curl and include cookie and conent-type header and send request to search.php .

```
curl -X POST -d '{"search":"london"}' -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' -H 'Content-Type: application/json' http://<SERVER_IP>:<PORT>/search.php
```

#### Using Fetch
- Copy request as Fetch, then go to **Console tab** and execute code.
- We receive same response as cURL.
---
### CRUD API
- Upto now, we used PHP parameters to search city.
- Will  now Utilize APIs for the same, and we will directly interact with the API endpoint.

## API
- **API** stands for **Application Programming Interface**.
- It is a **set of rules** that allows two software programs or systems to **communicate** with each other.
- Many APIs are used to interact with a database.
- To updatw city name using api.php
```
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london ...SNIP...
```

## CRUD
A **CRUD API** is an API that allows a client (like a browser or app) to **Create, Read, Update, and Delete** data on a server.


| Operation | HTTP Method | Description                                        |
| --------- | ----------- | -------------------------------------------------- |
| Create    | POST        | Adds the specified data to the database table      |
| Read      | GET         | Reads the specified entity from the database table |
| Update    | PUT         | Updates the data of the specified database table   |
| Delete    | DELETE      | Removes the specified row from the database table                                                   |
- Same principle is also used in REST APIs and several other types of APIs.

## Read
- First thing when interacting with API.
- Simply specify the table name after the API(e.g /city) and then specify our search term(e.g. /london)
   ```
   curl http://<SERVER_IP>:<PORT>/api.php/city/london


OUTPUT : ```
[{"city_name":"London","country_name":"(UK)"}]
- output is JSON string.
- To convert to JSON FORMAT , pipe the output to the **jq** utility. 
- To silent any unneeded cURL output, use -s flag.  

```shell-session
s0nam@htb[/htb]$ curl -s http://<SERVER_IP>:<PORT>/api.php/city/london | jq

[
  {
    "city_name": "London",
    "country_name": "(UK)"
  }
]
```

- To retrieve all entries , pass an **empty string**
- EG: curl -s http://<server_ip>:port/api.php/city/ | jq

---
## Create

- To post or add a new entry, use HTTP POST request.
- Set content-type header to JSON
   ```
   curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ -d '{"city_name:"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
``

- Try with fetch POST request in devtools.

---
## Update
- `PUT` is used to update API entries and modify their details.
- `DELETE` is used to remove a specific entity.
**Note:** The HTTP `PATCH` method may also be used to update API entries instead of `PUT`. To be precise, `PATCH` is used to partially update an entry (only modify some of its data "e.g. only city_name"), while `PUT` is used to update the entire entry. We may also use the HTTP `OPTIONS` method to see which of the two is accepted by the server, and then use the appropriate method accordingly.

   ```
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london -d '{"city_name:"New_HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
```
- Replaced old city with new.
**Note:** In some APIs, the `Update` operation may be used to create new entries as well. Basically, we would send our data, and if it does not exist, it would create it.

---
## Delete

```shell-session
curl -X DELETE http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City
```
