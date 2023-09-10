# Steps:

> Launch two instances in AWS, one for jenkins {master node} and other for docker {slave}.

# ON SLAVE (DOCKER NODE)
1. SSH and install docker by executing the file{script} downloaded by `wget https://get.docker.com` 
   on the agent node.
2. Also install openjdk-17-jre through `apt install -y openjdk`
3. Make sure that ubuntu user in the slave is able to perform the docker actions.
4. To permit ubuntu user to execute docker commands, add ubuntu to docker group 
   `sudo usermod -aG docker ubuntu`
5. To make sure that ubuntu user assumed the docker group permissions, restart the instance or
   execute `newgrp docker` and test with `docker container ps`
6. Now, Generate a key pair in the worker node for ubuntu user using `ssh-keygen` and cat the 
   public and private keys through `cat ~/.ssh/id_rsa.pub` and `cat ~/.ssh/id_rsa` respectively.
7. Append the public key in .ssh/authorized_keys file of the docker node.
8. Make sure that the dockerfile in method-1 directory is available in the home directory of the slave node.

# ON JENKINS NODE
1. Install jenkins LTS version from the official doccumentation.
2. Log in to the jenkins dashboard. {Make sure port: 8080 is open in the firewall.}
3. Under the credentials section in Manage Jenkins tab, add a SSH private key with username type
   global credential. Copy the private key [`cat ~/.ssh/id_rsa`] from the docker node and paste it here
   along with the username.
4. Now, create a new item (pipeline) and use the Jenkins file to provide the pipeline required to 
   execute this task. Paste the Source code http repo link as Github Project and also enable the option
   WebHook trigger by gitSCM polling to auto trigger the pipeline on recieving a delivery from the github 
   webhook.
5. Create a dummy repo and webhook `http:publicIP_of_jenkins:8080/github-webhook/`.
6. Push the Jenkinsfile and source code to this repo.
7. Now, whatever code we push to this repo will be automatically downloaded to the food1 directory and an
   image will be built from this code followed by launching a container.
8. As a result, we should be able to see this output in http://Slave_PublicIP:9999 {Makes sure port:9999 is open in firewall}
9. Test the functionality by changing the source code in the repo. {Jenkinsfile is a mandatory component.}

NOTE: Modify the Jenkinsfile as per the configuration and requirements.