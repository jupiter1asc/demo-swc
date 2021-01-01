# Deploy Jenkins with Docker, pipelines and blueocean plugin
### Setup Jenkins
To setup Jenkins you need to create `inventory` file (with the server IP) in the `Jenkins-Master` folder based on `inventory.example` and then run:
```shell script
 ansible-playbook --inventory inventory -u ec2-user --key-file jkey --verbose setup_jenkins.yml 
```
you should also add `jkey` ssh private key for `ec2-user` user in the `Jenkins-Master` folder.

### Github configuration (you could use any other VSC)
* Create a repo which has `Jenkinsfile` in the root folder of the project.

* Create a Personal access token for your Github user: https://github.com/settings/tokens

* Add webhook: `https://github.com/<repo-user>/<repo-nam>/settings/hooks`
```shell script
   Payload URL: http://<server-ip>:8081/github-webhook/
   Content type: application/x-www-form-urlencoded
   Which events would you like to trigger this webhook? : Pushes, Pull requests
   Active: yes
```
Be careful to use `http://<server-ip>:8081/github-webhook/`, not `http://<server-ip>:8081/github-webhook`

### Configure Jenkins
Jenkins URL: `http://<server-ip>:8081/`

Get the initial Jenkins password:
```shell script
sudo cat /root/jenkins/jenkins_data/secrets/initialAdminPassword
```
and install suggested plugins with the first login.

#### Create a new Jenkins Pipeline:
You need to add Github repo credentials for configuring the pipeline `http://<server-ip>:8081/blue/organizations/jenkins/pipelines`

### Configure SMTP with Gmail
Configure SMTP server parameters: `http://<server-ip>:8081/configure`
```shell script
SMTP server → smtp.gmail.com
Select Use SMTP Authentication
Put your gmail id
Put your gmail password
Use SSL select
SMTP port 465
```
If you faced with the mail sending issue, please do the next steps:
* Enable extended e-mail notification in Jenkins `Allow sending to unregistered users`
* Two Step Verification should be turned off: https://support.google.com/accounts/answer/1064203?hl=en
* Allow Less Secure App(should be turned on): https://myaccount.google.com/lesssecureapps

If you got the email sending error please check [this](https://forums.aws.amazon.com/thread.jspa?threadID=228587) and [that](https://issues.jenkins.io/browse/JENKINS-61073?workflowName=JNJira+%2B+In-Review&stepId=1) links.


### Troubleshooting
You might need `Approving signature` for example `staticMethod org.codehaus.groovy.runtime.DefaultGroovyMethods`

`http://<server-ip>:8081/scriptApproval/`
