# First Jobs
## Change admin password 
click on admin in a left upper right hand side of a screen  
Hit configure  
and change the password by pressing    

## First jobs
We are going to set up our first jobs    

## echo "Test" 
Go to jenkins, select New Items  
Type a name my_first_job, hit freestyle and OK      
select build Execute a shell   
type echo "test" 
and press  apply and save   
Press Build Now  
See the result by pressing the build #1  
and got to console output  

## install github plugin and maven plugin   
manage jenkins -> plugin manager -> tab available     
filter github and select github integration  
filter maven and select maven integration     
and hit install without restart  

go to manage jenkins -> global tool configuration 
Maven -> add maven    
Name Maven 3.6.3  
install automatically from Apache select version 3.6.3  
Hit apply and save

## Hello world using Maven 
New Item -> maven project and ok 
select git as a source 
https://github.com/crunchy-devops/hello-world.git  
Build uses a pom.xml file   
Goals are: clean install package 
Hit apply and save 
and press Build now 

## Hello world under Sonar 
manage jenkins -> manage plugins  tab Available     
filter sonar and select Sonarqube Scanner  
and install without restart  
Go to manage jenkins ->configuration system   
Sonarqube server   
Tick enable injection  
Name SonarQube  
ServerURL:  http://sonar:9000  
Create a credential jenkins   
in the credential screen select kind as secret text 
Open an other browser tab, go to  
Type ```http://<your_ip_address:19000>`` in your browser    
Login using user: admin  password : admin  
Go to administration, security , user, token  
Type a name and generate    
Copy the token in the credential as a secret text   
Set an ID text as SonarToken for this credential  
and a description  SonarToken   
Hit Add
 
Hit New Item,  enter a name hello-world-sonar
copy from  My_first_maven_build  
hit ok  
Tick in build environment 'Prepare SonarQube Scanner ...'    
Change Goals as ``` clean package sonar:sonar -Dsonar.host_url=$SONAR_HOST_URL```  

## How to check the code quality with Sonar
Type ```http://<your_ip_address:19000>``` in your browser  
Login using user: admin  password : admin  
See the result in the project  
 
## Deploy your war file to the repository Nexus
Open a browser tab :  
http://<your-ip_address:18081/  

get nexus password   
```docker exec -i jenkins-pic_nexus_1 cat /nexus-data/admin.password```

Go to your first maven build select configure   
and press post build actions  
select archive the artifacts, in the textedit files to archive type ```**/*.war```      
Build now this job again    
Go to manage jenkins -> Plugin Manager -> Tab available  
Filter copy artifact, check and 
Filter Nexus, select Nexus Platform,  check  
install without restart  
Go to manage jenkins ->configuration system     
find Sonatype Nexus  
Select 2.x Server   
Display Name :  Nexus  
Server ID :  Nexus  
Server URL: http://nexus:8081  
Create a credentials user/password  admin/xxxxx ID: nexuslogin   
Check with a Test Connection  
and then Apply and save
Create a job, freestyle as hello_world_nexus    
Go to Build, select copy artifacts from another project   
add an another build, Nexus Repository Manage Publisher  
Nexus instances:  Nexus  
Nexus Repo: Release   
add Package maven   
Group and artifact are coming from your project pom.xml  
Version: 1.1  
Packaging: war  
Add artifact Path:  
File Path:  webapp/target/webapp.war  
Apply and Save  
Build Now  
And check in Nexus http://<your_ip>:18081/  
Login admin and xxxxx 
Select in the left  Repositories, click on release and browser down to find your war file

## Get Nexus artifacts
curl -u admin:xxxx -X GET 'http://<ip_address>:18081/service/rest/v1/search/assets?repository=maven-releases&group=com.example.maven-project&maven.extension=war'
wget --user=admin --password=tcwowa12




