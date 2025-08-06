1. What is Jenkins?
- Jenkins is open source automation serverusedto build, test and deploy   appliations automatically as part of CI/CD pipelines

2. What is Jenkins mainly used for?
- Jenkins is mainly used for Continous Integration and Continous Delivery, Which automate the process of integrating code, testing it and delivering it to production and Staging environments

3. What is CI/CD and how does Jenkins fit into it?
- CI/CD stands for Continous integration and Continous Deployment. It means that whenever a developer pushes code to a repository, automated processes like build, test and deployment are triggered, recucing manual effort and human error


4. Can Jenkins only be used for code deployment?
- No, Jenkins is not limited to code deployment. It is aslo used for Continous Integration, Continous Delivery, Infrastructure automation, sheduled tasks, testing and even security scanning using plugins

# Installing Jenkins:

There are several way for installing Jenkins:

1. Locally using .war file (java required)
# Download Jenkins WAR
wget https://get.jenkins.io/war-stable/latest/jenkins.war

# Run Jenkins (Java 11 or above must be installed)
java -jar jenkins.war


2. Using system package (apt)

3. Inside a docker container

- docker run -d -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts

4. On cloud

# when Logged in to jenkins, Here are the key areas
1. New item (Create a new job) (Freestyle, pipeline, Multi-configuratin)
    1. Freestyle Job:
    - Pull code from a git repo
    - Run shell scripts or build commands
    - Archive artifacts
    - Trigger other jobs

# Creating first Job:
    Steps:
- Click New ietm from the Jenkins dashboard
- Name your Job
- Select "Freestyle Job" then ok
- In the Build sections, click "add build step" > Execute shell
- Add sample command "echo "Hello from Jenkins Freestyle Job"
- Click save and then build now from the left sidebar
- Click the build number #1 -> Console output to view the result

2. Build History: Shows past builds with results
3. Manage Jenkins: Configure system, plugins, security, credentials
4. Credentials: Store Git, DockerHub, SSH keys securely
5. Build Queue/ Executo: SHows active or waiting builds
6. People/My reviews: User Specific configurations (Useful in teams)

# Jenkins system configuration and Plugin Management
- Plugins: Plugins are backbone of Jenkins (Git, Docker, Maven, Slack etc)
- System settings control how jenkins behaves (email, build tools, agents)

# Key areas in Manage Jenkins:
- System Configuration: Global settings like tool locations, labels, environment variables
- Plugin Manager: Install/update/remove plugins
- Global tool configuration: Set up Java, Git, Maven, Gradle etc
- Security settings: Users, roles and credentials
- Nodes/Aegnts: Configure distributed builds
- System logs: View Jenkins activity and logs
- credentials: Store GitHub tokens, DockerHub logins, secrets securely


Tomorrow topics:

# Jenkins Pipelines
- What is Pipeline and why it is better than free style jobs?
- Scripted vs Declarative Pipelines?
- Create a simple Decalrative pipeline?
- Where to use Jenkinsfile?

