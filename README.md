# Jenkins CLI to Create and Build job

> This project is to create and build a Jenkins Jobs using Jenkins CLI. The project has two sample files: a sample Jinja template for jenkins job creation and a sample yaml file for values to be used in creation of jenkins job.

> #jenkins #jenkinscli #jenkinsjob #pipelinejob

---

## Jenkins CLI (In Action)

![jenkins cli](https://raw.githubusercontent.com/souravatta/Install-Configure/master/img/output.gif)

## Table of Contents

- [Jenkins CLI to Create and Build job](#jenkins-cli-to-create-and-build-job)
  - [Jenkins CLI (In Action)](#jenkins-cli-in-action)
  - [Table of Contents](#table-of-contents)
  - [Using Jenkins CLI for the First Time](#using-jenkins-cli-for-the-first-time)
    - [Requirements](#requirements)
    - [Downloading and Configuring](#downloading-and-configuring)
      - [Using the CLI over SSH](#using-the-cli-over-ssh)
      - [Using the CLI client](#using-the-cli-client)
  - [Usage](#usage)
  - [Example (pipeline.yaml)](#example-pipelineyaml)

---

## Using Jenkins CLI for the First Time

### Requirements
    ✔ JAVA (OpenJDK 8 or above)

    ✔ Jenkins (Version 2.204.0 or newer)

    ✔ Python (Version 3.0 or later)

    ✔ Pip (pip2 or pip3)

    ✔ Jinja2 (Latest version) ```snap install j2```


---

### Downloading and Configuring

The command line interface can be accessed over SSH or with the Jenkins CLI client, a .jar file distributed with Jenkins.

#### Using the CLI over SSH

* SSH service is disabled by default for new Jenkins installation.
* Go to Manage Jenkins → Configure Global Security. Under SSH Server, set the SSHD Port to Fixed (say 53801) or Random.

<a><img src="https://raw.githubusercontent.com/souravatta/Install-Configure/master/img/ssh_port.jpg?v=3&s=20" title="sshd-port" alt="sshd-port"></a>

* User used for authentication with Jenkins master must have Overall/Read permission in order to access CLI.
* Add SSH public key for the user by navigating to Manage Jenkins → Manage Users → User Settings.

<a><img src="https://raw.githubusercontent.com/souravatta/Install-Configure/master/img/ssh_pub_key.jpg?v=3&s=20" title="ssh-pub-key" alt="ssh-pub-key"></a>

* Execute CLI help command, to check ssh service for CLI:
```bash
ssh -l <user> -p <ssh-port> <hostname or ipaddress> help
```
**Example:** ```ssh -l ubuntu -p 53801 localhost help```

---

#### Using the CLI client

* Dowload the CLI client jar from jenkins master using url ```JENKINS_URL/jnlpJars/jenkins-cli.jar```
* General syntax for using the CLI client:
```bash
java -jar jenkins-cli.jar [-s JENKINS_URL] [global options...] command [command options...] [arguments...]
```
* There are three basic mode for client connection: ```-http, -websocket and -ssh```
* SSH connection mode syntax:
```bash
java -jar jenkins-cli.jar [-s JENKINS_URL] -ssh -user kohsuke command ...
```
**Example:** ```java -jar jenkins-cli.jar -s http://13.xxx.xxx.xxx:8080/ -ssh -user ubuntu help```


---
## Usage

> Step 1: Fork or Clone the repository [https://github.com/souravatta/Jenkins-CLI.git](https://github.com/souravatta/Jenkins-CLI.git)
> 
> Step 2: Download the [Jenkins CLI jar](#using-the-cli-client) inside the cloned repository.
>
> Step 3: Modify the values of **pipeline.yaml**
> 
> Step 4: Run the below command to create a jenkins job using Jenkins CLI.
```bash
j2 -f yaml pipeline.j2 pipeline.yaml | java -jar jenkins-cli.jar -s http://13.xxx.xxx.xxx:8080/ -ssh -user ubuntu create-job test6
```
> Step 5: Run the below command to build the jenkins job using Jenkins CLI.
```bash
j2 -f yaml pipeline.j2 pipeline.yaml | java -jar jenkins-cli.jar -s http://13.233.173.108:8080/ -ssh -user ubuntu build test6 -f -v
```
<a><img src="https://raw.githubusercontent.com/souravatta/Install-Configure/master/img/jenkins_cli.JPG?v=3&s=20" title="ssh-pub-key" alt="ssh-pub-key"></a>

**Note:** Plugins may also provide CLI commands; in order to determine the full list of commands available in a given Jenkins environment, execute the CLI help command:

```bash
ssh -l ubuntu -p 53801 localhost help
```
OR

```bash
java -jar jenkins-cli.jar -s http://13.xxx.xxx.xxx:8080/ -ssh -user ubuntu help
```

---

## Example (pipeline.yaml)

```yaml
pipeline_strategy:
build:
  allow_concurrent: false
  properties:
    days_to_keep: -1
    num_to_keep: 5
  parameters:
    - name: GREET_NAME
      value: Sourav
      description: Name of the person to greet
pipeline:
  git_url: https://github.com/souravatta/Install-Configure.git
  creds_id: github
  branch: master
  script_path: Jenkinsfile
```
