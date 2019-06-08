![](./images//Pivotal.png)

# Transforming How The World Builds Software
-----------------------------------------------------
## Workshop Agenda
(9:30AM to 3:30PM Breakfast & Lunch Included)

- Pivotal Mission & Applicability to our Customers
     - Value Statement
     - Case Studies
-  PCF Installation Steps
     - Operations Manager
     - Tile(s) Installation
- Demo & Hands-on
     - Developer Experience
        - PAS & PKS
     - Day 2 Operations
        - Scaling
        - Health Monitoring
        - Patching
        - Upgrading
     - Pivotal Container Service (Kubernetes) workload deployment
     - Pivotal Application Service for higher level developer productivity
- Pivotal Data Update and Q&A
- Next Steps
-----------------------------------------------------
## Technical Pre-Requisites
- A Mac or PC with internet connection, running a browser that can access https://chess.cfapps.io
- Ability to access and update this [Workshop Google Sheet](https://drive.google.com/open?id=1YcaNLkBqXHgYZch6yV8Kvf2G2AUG-trKSQQvejpstv8)
- Ability to SSH into a Ubuntu VM (public IP address) using a private-key file

  e.g. `ssh -i fuse.pem ubuntu@user1.ourpcf.com`
  - [fuse.pem](https://github.com/rm511130/PCF4CAS/blob/master/fuse.pem) must be set using `chmod 400` to read-only by owner
  - If using Powershell on a Windows machine read these [instructions](https://superuser.com/questions/1296024/windows-ssh-permissions-for-private-key-are-too-open)
  
-----------------------------------------------------

## Guidelines for this Workshop
- This workshop includes presentations, demos and hands-on labs. 
- The labs are interdependent and must be executed in order.
- Use the [Workshop Google Sheet](https://drive.google.com/open?id=1YcaNLkBqXHgYZch6yV8Kvf2G2AUG-trKSQQvejpstv8) to claim a user-id for this workshop. For example, Ralph Meira is user1.
- Update the [Workshop Google Sheet](https://drive.google.com/open?id=1YcaNLkBqXHgYZch6yV8Kvf2G2AUG-trKSQQvejpstv8) as you progress through the Labs, by placing an "x" in the appropriate column.
- When carrying out hands-on labs, you can simply cut-&-paste the commands shown `in boxes like this one`. 
- However, when issuing commands, please make sure to alter the user-id to match the one you have claimed, e.g.:
  - `ssh -i fuse.pem ubuntu@user3.ourpcf.com` is for `user3` 
  - `ssh -i fuse.pem ubuntu@user15.ourpcf.com` is for `user15`
- Don't get stuck. Ask for help. The goal is to learn concepts and understand how Pivotal can help CAS be successful.
- The PAS and PKS platforms we will be using were created using self-signed certificates, so you will sometimes have to click through warning screens about insecure pages.
- Each workshop participant will be assigned an Ubuntu VM on AWS which has been readied for the execution of hands-on Labs. Your Laptop or Desktop will be used for two purposes: 
     - SSH'ing into the Ubuntu VM 
     - Browsing web pages

-----------------------------------------------------

## Pivotal's Value Statements
- Enterprises rely on software to improve business outcomes.
- They depend on velocity of converting ideas to new features, new applications, new capabilities, and new services.
- The pace of change is generating a daunting backlog of work for IT leaders, who are also under pressure to reduce technology spend and limit security vulnerabilities.
- The competing mandates - velocity, security, and operational efficiency - force enterprises to re-evaluate how they develop, architect, and operate software. 
- Pivotal helps enterprises accelerate their transition to continuous delivery, reducing waste (costs and time) through process and automation to achieve world-class efficiency and productivity.
- Pivotal’s customers are achieving are compelling Business Outcomes.

-----------------------------------------------------

![](./images/5s.png)

-----------------------------------------------------

## Case Studies

World-class teams of agile developers, product managers, and designers can adopt the [Pivotal way](https://pivotal.io/labs) of building and deploying software with quality and sustainable velocity in a cost-effective manner. The techniques Pivotal uses and advocates, involve Minimum Viable Products (MVPs), Lean experiments, Identifying & testing assumptions, Data driven decisions, User-centered designs, Prototyping, Pair Programming, Test-Driven Development (TDD), Short Iterations, Continuous Integration and Deployment.

The results are amazing:
- Building working software at a consistent speed and quality in the face of changing requirements.
- Ensuring the software solves a real problem for real users in a desirable and usable product.
- Reducing the risk of building the wrong thing while comfortably changing direction.


https://pivotal.io/customers

-----------------------------------------------------

## PCF Architecture, Installation & Set-up 

- [Containers 101](https://drive.google.com/open?id=1vRisBwfNmD22o_d7OC_OWS9g-M2b2p_q)

![](./images/CaaS_PaaS_FaaS_OSBAPI.png)

- [PKS 1.4 on vSphere](https://drive.google.com/file/d/1Jwytpm-kO0trS-5vAUKRssSVCXv98G0o/view)


-----------------------------------------------------

### LAB-1: SSH into your Linux Workshop environment & test the Command Line Interface tools

Let's start by logging into the Workshop environment from your machine (Mac, PC, LapTop, Desktop, Terminal, VDI). You will need to use the following private key: [fuse.pem](https://github.com/rm511130/PCF4CAS/blob/master/fuse.pem).

```
ssh -i ./fuse.pem ubuntu@user1.ourpcf.com
```

Once logged in, execute the following commands:

```
pks --version
```

```
kubectl version
```
If you see a connection refused message, don't worry, it is expected and not a problem.

```
cf --version
```

```
git version
```

If all the commands shown above displayed their respective CLI versions, you have successfully completed Lab-1.

Please update the [Workshop Google Sheet](https://drive.google.com/open?id=1YcaNLkBqXHgYZch6yV8Kvf2G2AUG-trKSQQvejpstv8) with an "x" in the appropriate column.

Note: if you had to install the pks, kubectl and cf CLIs, you would need to download the binary files from [PivNet](http://network.pivotal.io) and place them under `/usr/local/bin` using `chmod +x` to make them executable.

-----------------------------------------------------

### LAB-2: Connecting to PCF PAS (Pivotal Application Service) 

Pivotal Application Service is the PaaS (Platform as a Service) component of the Pivotal Cloud Foundry ecosystem. It is the best place for developing and running applications (incl. Java, .NET, Spring, Node.js, Go, ...). PAS can be deployed on all major IaaS solutions (vSphere, OpenStack, GCP, AWS and Azure) to deliver a simple and consistent interface to Developers (Apps Manager, CF CLI) and Operators (Ops Manager, CF CLI, Bosh).

![](./images/pas_diagram.png)

Assuming you completed LAB-1 successfully, you should be logged into an Ubuntu VM hosted on AWS. Using this VM, change the User_ID in the following command and execute it to connect to PCF/PAS (Pivotal Application Service) - make sure to use the correct User-ID, i.e. the one claimed in the [Workshop Google Sheet](https://drive.google.com/open?id=1YcaNLkBqXHgYZch6yV8Kvf2G2AUG-trKSQQvejpstv8) 

```
cf login -a api.sys.ourpcf.com --skip-ssl-validation -u user1 -p password      # change -u userX to match your User-ID
```

You will land in an ORG and SPACE that were created just for you to use and manage during this workshop. ORGs are often used to isolate Business/App-Programs/Products, and SPACEs are used to isolate Apps in DEV, TEST, and PROD phases. Let's continue:

```
cf create-space development
cf orgs; cf spaces
cf target -s development
```

Grant a colleague of yours access to your `development` space. In the example below we'll use `user2` but you can pick anyone participating in this workshop.

```
cf set-space-role user2 org1 development SpaceDeveloper
cf space-users org1 development
```

Let's recap: You have access to PAS (Pivotal Application Service), to an ORG and two spaces that can be used for your projects. You can develop code in the developement SPACE and promote it into the production SPACE.

Congratulations, you have completed LAB-2. 

-----------------------------------------------------

### LAB-3: The Developer's Haiku - "Here is my source code, Run it on the cloud for me, I do not care how."

Let's continue from Lab-2 by grabbing some code from github. Continue using your Ubuntu VM.

```
cf target -s development
cd ~    # just to make sure you are in your home directory
git clone https://github.com/rm511130/chess; cd chess; ls -las
```

Your code is in the `cat index.php` file. It's a ♞ Chess Game written in Javascript.
The `cat manifest.yml` states that your code requires 100MB of RAM and a random URL to avoid collision with other colleagues.

Let's `cf push` your Chess App.

```
cf push
```

Once the process has been completed, you should look for the random route URL that PAS created for you. The output will look something like this:

```html
name:              chess
requested state:   started
routes:            chess-intelligent-oryx.apps.ourpcf.com
last uploaded:     Fri 07 Jun 18:02:10 UTC 2019
stack:             cflinuxfs3
buildpacks:        php 4.3.70
```

Access your route / URL or ask someone to access it. You should see someything similar to this:

![](./images/chess.png)

Let's recap: You have deployed a Chess App into the cloud, without having to worry about IP addresses, ports, middleware, containers, VMs, network routers, application routes, DNS entries, app logging, app performance monitoring, firewalls, etc., and you didn't have to open a service ticket. 

You can easily scale horizontally by increasing the number of containers running your Chess App, and you can log into any specific container. Let's try this:

```
cf scale chess -i 3
cf ssh chess -i 2
set   
exit
```

Let's recap: You just scaled from 1 to 3 (not a limit) containers running the same Chess App. PAS provided automatic loadbalanced routing across all three containers, and you accessed the 3rd container (by using `-i 2`) which is useful for debugging specific issues to any given container type or container instance.

Congratulations, you have completed LAB-3.

-----------------------------------------------------

### LAB-4: Apps Manager & Ops Manager

Time is short so we will demonstrate the features of Ops Manager, the operators GUI, `http://opsman.ourpcf.com` to the workshop participants:

![](./images/OpsMan.png)

You can access Apps Manager, the developers GUI, `http://login.sys.ourpcf.com` using your User# and `password` for password.

![](./images/AppsMan.png)

We will also demonstrate the features of Apps Manager, but you are welcome to click around and learn about its features: events, services, routes, tasks, logs, traces, threads, settings, (auto)scaling, metrics, life-cyle management and health-management.

Congratulations, if you accessed Apps Manager successfully, you have completed Lab #4.

-----------------------------------------------------

### LAB-5: PAS handles Docker Images and it's the best PaaS for running Spring Apps

Assuming that your are logged into your Ubuntu Workshop VM proceed as follows:

```
cf push factorial --docker-image rmeira/factorial --random-route 
```

Once deployed, you can test `factorial` by using a browser or a `curl` command per the example below:

```
curl factorial-courteous-toucan.apps.ourpcf.com/6; echo
```

You should see results similar to this: 

![](./images/docker.png)


Let's `cf push` a Spring Boot application:

```
cd ~   # to make sure you are back to the your home directory
git clone https://github.com/rm511130/spring-music
cd spring-music
./gradlew clean assemble
cf push
```

PCF PAS uses its Java Buildpack to create a container with all the dependencies necessary to run your Spring-Music App.

![](./images/SpringMusic.png)

Go back to [Apps Manager](http://login.sys.ourpcf.com) and take a look at how it has recognized and reconfigured itself for the Docker and Spring Boot Apps. 

Congratulations, you have completed Lab-5.

-----------------------------------------------------

### LAB-6: Installing the PKS Tile in Ops Manager

- K8s (Kubernetes) is an open-source platform for building platforms. It is a system for automating the deployment, scaling, and management of containerized applications. 
- Pivotal Container Service (PKS) enables operators to provision, operate, and manage enterprise-grade Kubernetes clusters using BOSH and Pivotal Ops Manager. N
- Neither of the two is a PaaS.

![](./images/k8s_and_pks.png)

During this workshop we will show you many the key aspects of the installation and set-up of the PKS Tile and the creation and management of K8s clusters. However, installimg PKS on any IaaS takes at least 1hr, so please watch this compressed [15min video](https://www.youtube.com/watch?time_continue=83&v=Oxw-lucgpX0) to get an idea of the steps involved.

![](./images/bosh_pks_k8s_on_aws.png)

The major steps in getting to the picture shown above are:

1. Set-up the networking infrastructure: e.g. install NSX-T or run Terraform scripts on your IaaS to create load balancers, reserve subnets, establish firewalls, service accounts with the correct permissions, etc.
2. Download the Ops Manager VM bits or Image Metadata from [PivNet](https://network.pivotal.io) and install it on your IaaS
3. Set-up Ops Manager Director: creating Availability Zones, Networks, etc.
4. Download PAS and PKS Tiles from [PivNet](https://network.pivotal.io), import them into Ops Manager, add, set-up and apply.



- [Awesome K8s](https://ramitsurana.github.io/awesome-kubernetes/)















