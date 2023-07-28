Table of contents
----------------

1. Gitlab CI setup with Azure	
1.1 Install the gitlab runner by using binaries.	
1.2 Gitlab runner executor, how to install/configure executor.	
1.1.3 Docker setup in VM	
1.1.4 If you want to update the GitLab executor. Try this!	
2. Reference:	
----------------------------------------------------------------------------------------------------------------
1. Gitlab CI setup with Azure 

	1.1 Install the gitlab runner by using binaries. 

Simply download one of the binaries for your system:

**sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64
**


Give it permissions to execute:

**sudo chmod +x /usr/local/bin/gitlab-runner
**


Create a GitLab CI user 

**sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
**

Install and run as service:

**sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
**
**sudo gitlab-runner start
**

Let’s, check if the runner is running or not. 

**sudo gitlab-runner status

**Let’s also check the service folder,

---------------------------------------------------------- 
cat /etc/systemd/system/gitlab-runner.service
[Unit]
Description=GitLab Runner
ConditionFileIsExecutable=/usr/local/bin/gitlab-runner
After=syslog.target network.target
[Service]
StartLimitInterval=5
StartLimitBurst=10
ExecStart=/usr/local/bin/gitlab-runner "run" "--working-directory" "/home/gitlab-runner" "--config" "/etc/gitlab-runner/config.toml" "--service" "gitlab-runner" "--user" "gitlab-runner"
Restart=always
RestartSec=120
EnvironmentFile=-/etc/sysconfig/gitlab-runner
[Install]
WantedBy=multi-user.target
---------------------------------------------------------------------------------------------------------------------

Now, we are going to add this runner into gitlab runner. For that we must register this runner first. 

1.2 Gitlab runner executor, how to install/configure executor.

From the given screenshot, check the executor section mentioned, there are various options available. Check the official documentation here.
Above section, I have chosen ‘docker as gitlab runner executor. That means, github runner will try to authenticate by using docker to login to the server. We can also set up ssh,shell,Kubernetes as runner depends upon the requirement and feasibility. 

1.1.3 Docker setup in VM
 
Follow the official documentation to install docker


1.1.4 If you want to update the GitLab executor. Try this!
--------------------------------------------------------------

Stop the service (you need elevated command prompt as before):
sudo gitlab-runner stop


Download the binary to replace the GitLab Runner executable. For example:
sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64"

You can download a binary for every available version as described in Bleeding Edge - download any other tagged release.

Give it permissions to execute:
sudo chmod +x /usr/local/bin/gitlab-runner
Start the service:
sudo gitlab-runner start


2. Reference: 

https://docs.gitlab.com/runner/install/linux-manually.html
https://docs.docker.com/engine/install/ubuntu/




Install and run as service:

sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner

sudo gitlab-runner start






