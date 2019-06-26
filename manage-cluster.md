##### https://github.com/ronakbanka/manage-pks by Ronak Banka

# Creating PKS K8s Clusters using Manage-Cluster Utility

Assuming you have a PKS environment running on AWS accessible via:

```
pks login -a https://api.pks.ourpcf.com:9021 -u pks_admin -p password -k
```

You can execute the following steps to install `manage-cluster` on your Mac:

```
$ git clone https://github.com/ronakbanka/manage-pks
$ cd manage-pks/aws
```

Let's make sure my `aws cli` is working:

```
$ aws configure
AWS Access Key ID [****************JETA]: 
AWS Secret Access Key [****************4Yfh]: 
Default region name [us-east-1]: 
Default output format [json]: 
```

A quick way to test your `aws cli`:

```
aws ec2 describe-instances | grep -i privateipaddress
```

# Now for the fun part: Creating a PKS Cluster    

Remember to make sure you are in the correct directory:

```
cd /work/manage-pks/aws
$ ./manage-cluster 
Usage: ./manage-cluster {provision|access|cleanup}
```

OK, then lets create a cluster:

```
$ ./manage-cluster provision
```
```
Enter pks cluster name to be created and press [ENTER]:
one
```
```
Listing pks plans...

Name    ID                                    Description
small   8A0E21A8-8072-4D80-B365-D1F502085560  Example: This plan will configure a lightweight kubernetes cluster. Not recommended for production workloads.
medium  58375a45-17f7-4291-acf1-455bfdc8e371  Medium sized k8s cluster, suitable for more pods.
```
```
Enter pks plan to be provisioned, choose plan from above list and press [ENTER]:
medium
```
```
Fetching domain from provided hosted zone

Creating PKS cluster one with external hostname one-k8s.ourpcf.com. 

Name:                     one
Plan Name:                medium
UUID:                     f79a6c2e-d6ca-4e4e-aa48-aa23909407f6
Last Action:              CREATE
Last Action State:        in progress
Last Action Description:  Creating cluster
Kubernetes Master Host:   one-k8s.ourpcf.com.
Kubernetes Master Port:   8443
Worker Nodes:             3
Kubernetes Master IP(s):  In Progress
Network Profile Name:     

Use 'pks cluster one' to monitor the state of your cluster
```

So notice that the process flows quickly and ends by telling you to use `pks cluster one` to get updates.

```
$ pks cluster one

Name:                     one
Plan Name:                medium
UUID:                     f79a6c2e-d6ca-4e4e-aa48-aa23909407f6
Last Action:              CREATE
Last Action State:        in progress
Last Action Description:  Instance provisioning in progress
Kubernetes Master Host:   one-k8s.ourpcf.com.
Kubernetes Master Port:   8443
Worker Nodes:             3
Kubernetes Master IP(s):  In Progress
Network Profile Name: 
```

Note that the `one-k8s.ourpcf.com` was created, however there's no AWS Load Balancer, Route53 Entry or Security Group just yet. Once we see the `Last Action State: Succeeded` message we can continue.












