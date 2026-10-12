**Definition:**  
Access control enforces _who can do what_ on an application or resource.

**Core Dependents:**

- **Authentication** – Confirms identity

- **Session Management** – Tracks user across requests

- **Authorization** – Enforces permissions
    

---

## **Broken Access Control**

- Occurs when users can access resources or actions _they shouldn’t_.
    
- Common critical vulnerability in web apps.
    
- Caused by flawed logic, misconfiguration, or missing checks.!


![[Pasted image 20250726194049.png]]

---

### Access Control Security Models

**Definition:**  
A structured way to define rules that control user access to resources, independent of platform or technology.

---

### Main Models:

1. **Programmatic Access Control**
    
    - Access rules stored in a database
        
    - Enforced by code
        
    - Supports roles, users, workflows
        
    - Highly flexible and granular
        
2. **Discretionary Access Control (DAC)**
    
    - Resource owners assign permissions
        
    - Fine control per user/resource
        
    - Can become complex to manage
        
3. **Mandatory Access Control (MAC)**
    
    - Central authority controls access
        
    - No user-based permission changes
        
    - Used in high-security systems (e.g., military)
        
4. **Role-Based Access Control (RBAC)**
    
    - Access based on user roles
        
    - Easier to manage than DAC
        
    - Effective when roles are well-defined
        
    - Scales well in organizations
        

---
### Vertical Access Controls 

**Definition:**  
Controls that restrict access to **different functionalities** based on **user roles or levels**.

**Example:**  
Admins can edit or delete users; regular users cannot.

**Purpose:**

- Enforce **role-based** restrictions
    
- Support **separation of duties** and **least privilege** principles
    
- Often implemented using access control models like RBAC
    

**Key Point:**  
Different users = different permissions based on their role or level.

---
### Horizontal Access Controls

**Definition:**  
Controls that restrict users to accessing **only their own resources** of the same type.

**Example:**  
A user can view **only their own** bank account, not others’.

**Purpose:**

- Prevent users from accessing other users’ data
    
- Common in multi-user applications
    
- Enforces **user-level data isolation**
    

**Key Point:**  
Same role or privilege, but **restricted to own data** or resources.

---
Here’s a short **quick recap** for **Context-Dependent Access Controls**:

---

### Context-Dependent Access Controls 

**Definition:**  
Access is restricted based on the **state of the application** or **user’s previous actions**.

**Example:**  
Users can't change the shopping cart **after payment** is made.

**Purpose:**

- Enforce correct **action sequences**
    
- Prevent misuse of functionality due to **incorrect state**
    
- Supports business logic and workflow enforcement
    

**Key Point:**  
Access depends on the **current context** or **sequence of events**, not just user role or identity.


---

### Broken Access Controls – Quick Recap

**Definition:**  
Occurs when users can access data or functions they’re **not authorized** to.

---

### Key Examples:

1. **Vertical Privilege Escalation**
    
    - User gains access to **higher-privileged** functions (e.g., admin page)
        
    - Example: Regular user deletes other accounts via admin panel
        
2. **Unprotected Functionality**
    
    - Sensitive functions exist but lack proper access checks
        
    - Example: Admin page accessible by directly visiting `/admin` URL
        
    - Common reasons:
        
        - Relying only on UI to hide links
            
        - Sensitive URLs exposed in `robots.txt` or guessed via brute-force
            

**Key Point:**  
Access control must be enforced **server-side**, not just hidden in the interface.

---
