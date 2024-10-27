---
title: Setting up Selenium Grid on Docker AWS
author: Bash
pubDatetime: 2022-09-18T01:00:00Z
slug: Selenium
featured: true
draft: false
tags:
  - Docker
  - AWS Cloud
  - Selenium
  - DevOps
ogImage: ""
description: Selenium Grid docker walkthrough

---
## Setting Up Selenium Grid 

Welcome! Let’s go about how you can create a Selenium grid on AWS. First off, let’s go about what a Selenium grid is and why you’d want to create one. A Selenium grid is a node that allows you to connect to one or more browsers. It acts as a proxy server that you can use to test your code on multiple browsers in parallel.

Enough theory, let’s get practical! First off you need an AWS account that you can create for free. AWS provides you with instances called Elastic Cloud Compute(ec2) that act as a virtual machine on the cloud. We will create an ec2 instance and install docker in it. Navigate to the launch instances section in your AWS. Select an Amazon Machine Image(AMI) that acts as your “iso file" in the cloud. Select the free tier eligible image. This is important because select any other image will raise a billing. We don’t want that to happen, we are broke!

Select a free image and create a new key pair. A key pair in AWS will allow you to connect to your instance remotely via ssh through. Connecting to your instance adopts an encryption similar to public key encryption so you need both keys for authentication. Make sure you download it in an accessible folder. I’ll save mine as grid.pem.Check that SSH is open on port 22 and accessible from anywhere. On the security group section, click the security group link and click edit the security rules. Add a new rule on inbound traffic that will allow traffic on port 4444 and give it a custom TCP. Make it accessible from MyIP only so that no one else can access your grid. Note that selenium grid listens to port 4444. Finally, launch your instance.

Now go to your PowerShell or Terminal, open the directory where you saved your key pair.

On the linux terminal, make your key pair executable and give it root access.

sudo chmod 400 grid.pem

sudo grid.pem +x

On PowerShell, you should be ashamed!

#to reset permissions
icacls.exe grid.pem /reset
#give it run permissions
icacls.exe grid.pem /grant:r “$($env: username): (r)”

#### remove inherited permissions

icacls.exe grid.pem /inheritance:r

Now that your key pair is all set let’s connect to your ec2 instance through ssh. Go to your Aws running instances select the instance you set up, click connect. Navigate to the SSH option and copy the connect command which should look something like this:

ssh -i “grid.pem” ec2-user@ec2-3.9.0.174-amazon-aws

The ec2-user is the default user but it is recommended to check if your AMI uses a different one.

Now in our ec2 instance, let’s sudo yum update first to make sure everything is upto date. Install the latest version of docker and test if it is running.

Let’s activate the docker services with sudo service docket start. Change the usermod we are running on with the sudo usermod -a -G docker ec2-user command. Finally restart the terminal/PowerShell to reset the permissions.

There you have it docker is up and running on your ec2 instance. Test this by running the docker ps command to check if any containers are available. There should be none.

#Coffee Break + ads

Let’s install the Selenium grid on our docker. We have two options on doing this;

    Manually connect and run selenium nodes and hub
    Use a docker-compose file that will do all the work for us

I’ll explain both starting with the manual way. Start by installing selenium on docker bind it to port 4444 and add the browser images we want to use as containers. I’ll add the command for chrome-browser and firefox browser.

$ docker network create grid
$ docker run -d -p 4444:4444 --net grid --name selenium-hub selenium/hub:3.11.0-bismuth
$ docker run -d --net grid -e HUB_HOST=selenium-hub -v /dev/shm:/dev/shm selenium/node-chrome:3.11.0-bismuth
$ docker run -d --net grid -e HUB_HOST=selenium-hub -v /dev/shm:/dev/shm selenium/node-firefox:3.11.0-bismuth

This images can be found in the official docker/selenium documentation https://hub.docker.com/u/selenium/

To add more nodes to your grid you can reuse the browser image and simply paste it again on the terminal.

This can be very tiring especially if you want many different nodes and managing them becomes alot of work. As techies we are by nature lazy so let’s make something that will automate all this for us. Enter the docker-compose <drumrolls>

The docker compose comes with a docker-compose.yml file that contains all the images we want and different functionalities we’d like our selenium to have.

You can get the latest docker-compose by either pip install the latest version from GitHub or using the wget on the terminal.

wget https://github.com/docker/compose/releases/download/<version>/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

chmod 400 usr/local/bin/docker-compose

Notice that we have set it in the usr/local/bin/ dir so we give it permissions. Test that your docker-compose works by checking the version with docker-compose --version .

Great now for let’s get our wand and do some magic. Here our wand will be the docker-compose.yml/yaml file.

Create a new file and call it docker-compose.yml using the touch command.

touch docker-compose.yml

Go to the SeleniumHQ site and and copy a docker compose file and paste it on your file using the nano or vim editor. You can also write your own based on your requirements. Check that the number of image replicas is as you want. Also check that the restart service is set to always. This is necessary or you’ll have to restart the docker containers everytime [PTSD intensifies].

Check your docker-compose.yml is available using ls command.

Now for the magic word, say it with me three times

docker-compose up

There you have it everything is ready, let’s now go back to your AWS tab. Select your ec2 instance and get the public IP address.

Go to another tab and type your public url with port 4444

Http://x.x.x.x:4444

You should see your Selenium grids on the console section.

We are done! You can get the whole url http://x.x.x.x/grid/console and add it to your code to test it on multiple browsers.

Thank you for reading through. Happy grinding ☺