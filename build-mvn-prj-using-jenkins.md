```
------------------------------------------------------
Install Git
------------------------------------------------------
sudo dnf install -y git

------------------------------------------------------
Install Maven - https://maven.apache.org/download.cgi
------------------------------------------------------
wget https://dlcdn.apache.org/maven/maven-3/3.9.14/binaries/apache-maven-3.9.14-bin.tar.gz

sudo tar xvf apache-maven-3.9.14-bin.tar.gz -C /opt

# Create a symlink to the maven folder when you have a new version of maven, you just have to update the symbolic link and the path variables remain the same

sudo ln -s /opt/apache-maven-3.9.14 /opt/maven

# Add Maven folder to System PATH

- To access the mvn command systemwide, you need to either set the M2_HOME environment variable or add /opt/maven to the system PATH.

- We will do both by adding them to the profile.d folder. So that every time the shell starts, it gets sourced and the mvn command will be available system-wide.

1) Create a script file named maven.sh in the profile.d folder:

sudo vi /etc/profile.d/maven.sh

2)  Add the following to the script and save the file:

export M2_HOME=/opt/maven
export PATH=${M2_HOME}/bin:${PATH}

3) Add execute permission to the maven.sh script:

sudo chmod +x /etc/profile.d/maven.sh

4) Source the script for changes to take immediate effect

source /etc/profile.d/maven.sh

5) Verify maven installation

mvn --version
	

------------------------------------------------------
Install Jenkins Plugins
------------------------------------------------------
Maven Integration

------------------------------------------------------
Save the Maven location on Jenkins
------------------------------------------------------
Manage Jenkins >> Tools >> MAVEN_HOME = /opt/maven

------------------------------------------------------
Create a Jenkins Job (freestyle)
------------------------------------------------------

- Create a new freestyle job with build step "Invoke Maven top level targets"
- Select Maven version >> clean validate

```
