
# Hygieia installation instruction

Here is the [Hygieia documentation](https://hygieia.github.io/Hygieia/setup.html)

## Check prerequisites
- Git
- Java (1.8 and above)
- Maven (3.3.9 and above)

```bash
# Installing maven
brew update
brew install maven
```


## Install MongoDB


- Install MangoDB Community Edition
```bash
brew tap mongodb/brew
brew install mongodb-community@4.0
```

- Create Data Directory and Change directory of data storage 
```bash
mkdir -p Documents/dev/data/db
cd /usr/local/var/mongodb
mongod --dbpath /Documents/dev/data/db
```

- Start MongoDB
```bash
brew services start mongodb-community@4.0

mongo

#switch to db dashboard
use dashboarddb

#Create db user
db.createUser(
			{
			 user: "dashboarduser",
			 pwd: "dbpassword",
			 roles: [
			 {role: "readWrite", db: "dashboarddb"}
			 ]
			})

#exit mongo shell
exit
```

## Build Hygieia

- Run Maven Build for each project
```bash
#go to Hygieia and hygieia-core and run
mvn clean install package
```


## Encryption for Private Repos

```bash
cd Documents/hygieia-core/target
java -jar /Users/username/Documents/hygieia-core/target/core-3.1.1-SNAPSHOT.jar com.capitalone.dashboard.util.Encryption
```
- Copy key value and add the key to the api.properties file
```bash
key=<your-generated-key>`
```
- Copy key value and add the key to the bitbucket.properties file
```bash
git.key=<your-generated-key>`
```


## Setup API
-  Set parameters in api.properties
- Run API
```bash
cd Documents/Hygieia/api/target/

#change to your location
java -jar api.jar --spring.config.location=/Users/username/Documents/Hygieia/api/src/main/resources/api.properties -Djasypt.encryptor.password=hygieiasecret  
```


## Setup UI

The Hygieia dashboard requires installation of:
-   NodeJS
-   npm
-   gulp


```bash
brew install node
npm install
sudo npm install bower -g
sudo npm install -g gulp
```

- Run UI

```bash
# New terminal tab
cd Documents/Hygieia/UI
gulp serve
```

## Setup collectors

**Jenkins**
- Set parameters in jenkins.properties file 
- Run Jenkins
```bash
# New terminal tab
cd Documents/Hygieia/collectors/build/jenkins/target/
java -jar jenkins-build-collector-3.0.2-SNAPSHOT.jar --spring.config.name=jenkins --spring.config.location=/Users/username/Documents/Hygieia/collectors/build/jenkins/jenkins.properties
```
In order to keep Hygieia updating, all collectors tabs should be opened 


**Bitbucket** 
- Add repo clone URL in Repo URL widget setting


