able to access application "spring-petclinic-2.4.2.jar" from http://3.25.55.233:8080
Steps followed:
Step I: Create an ubuntu linux vm and login into that
Step II: Verify Java, if not install jdk 11
Step III: Now download the application in tmp location
Step IV: Now run the application
```
cd spring-petclinic
mvn package # creates package, run unit tests
```
Step V: Now access the application in browser
Step VI: Created a Inbound rule: port 8080
# Manual steps
sudo apt update
sudo apt install openjdk-17-jdk maven -y
git clone https://github.com/spring-projects/spring-petclinic.git
cd spring-petclinic
mvn package # creates package, run unit tests
