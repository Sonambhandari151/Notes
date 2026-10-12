


## LEVEL 
0. ssh username@hostname -p port
1. cat readme
2. cat ./- (to read hyphen files)
3. cat spaces\ in\ this\ filename
4. cd inhere,   ls -al,   cat ...Hiding-From-You
5. ls -a,  cd inhere,   ls -a,   file ./-*,    cat ./-file07
6. ls , cd inhere,  find -type f -size 1033c, cat ./maybehere07/.file2
7. find / -type f -user bandit7 -group bandit6 -size 32c, cat /var/lib/dpkg/info/bandit7.password
8. grep millionth data.txt( grep to search)
9. sort data.txt |uniq -u
10. strings data.txt |grep ^=*
11. base64 -d data.txt
12. cat data.txt | tr A-Za-z N-ZA-Mn-za-m
The command `chmod 644` is used to modify the **permissions** of a file or directory in Unix-like operating systems. Here's what it does:

---

### **Understanding File Permissions**

In Unix-like systems, file permissions are represented as a set of three groups:

1. **Owner (User)**: The person who owns the file.
2. **Group**: A group of users who can access the file.
3. **Others**: Everyone else.

Permissions can include:

- **Read (r)**: The ability to read the contents of the file.
- **Write (w)**: The ability to modify the file.
- **Execute (x)**: The ability to execute the file as a program/script.

Each permission is represented numerically:

- `4`: Read
- `2`: Write
- `1`: Execute
- `0`: No permission

---

### **What Does `chmod 644` Do?**

The **644** sets permissions as follows:

1. **Owner (6)**:
    - `4 + 2 = 6` → Read and Write permissions.
2. **Group (4)**:
    - `4` → Read-only.
3. **Others (4)**:
    - `4` → Read-only.

So, **chmod 644** gives:

- **Owner**: Read & Write.
- **Group**: Read-only.
- **Others**: Read-only.

/tmp/tmp.LkSFsnZdnN
- **`>`**: Overwrites the file with new data (replaces content).
- **`>>`**: Appends new data to the file without deleting existing content.