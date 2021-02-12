# **Digital.ai Deploy demo with Ansible**


This a demo setup to demonstrate a sample use case of using Ansible with Digital.ai Deploy.
The demonstration works on a local machine and requires an internet connection (to provision the machines).

## Overview:

The demo architecture is based on 4 machines: 

* The host machine: where Deploy is installed
* Machine 1 called 'Ansible controller machine': where ansible is installed
* Machine 2 called 'Ansible target machine 1': where to apply Ansible playbook and deploy application.
* Machine 3 called 'Ansible target machine 2': same as machine 2.

## Architecture before the deployment:
![before deployment](images/before.png)

## Architecture after the deployment:
![after deployment](images/after.png)

All the machines are configured on the host machine using vagrant.

* Machine 1: fixed IP : 192.168.78.3
* Machine 2: fixed IP : 192.168.78.4
* Machine 3: fixed IP : 192.168.78.5

If you want to change the IP address edit both the 'machines/Vagrantfile' and 'gitops/Infrastructure.yaml'

# Configuration steps before running the demo

## Step 1/5 : Change the key paths
Change the absolute path to the keys, in the file 'gitops/Infrastructure.yaml'

If you want to generate new keys run the following command and change the file 'gitops/Infrastructure.yaml' and the file 'machines/Vagranfile'

```
ssh-keygen
```


## Step 2/5 : Start the machines
In the machines folder run the command:
```
vagrant up
```
to create and start the 3 machines.
It may take a few minutes.


## Step 3/5 : Check or Change the Deploy URL
In the file 'gitops/Infrastructure.yaml', change the value for the key 'devopsAsCodeUrl'.
It's the URL to the local Deploy server. Default value is "http://localhost:4516"

## Step 4/5 : import items in Deploy
In the gitops folder run the command:
```
xl apply xl-deploy -f AnsibleDemo.yaml
```

## Step 5/5: start a deployment

Deploy the app 'Applications/AnsibleDemo/java-server-application/1.0.0' in the environment 'AnsibleDemo/Local/AnsibleLocal'



# During the demo
The default deployment does not use any orchestrator. All the tasks are executed sequentially.

![default deployment](images/defaultDeployment.png)

To demonstrate one of the value of Deploy, undeploy the app and do a second deployment with the orchestrator 'parallel-by-container'. Now the provisonning for each machine is done in parallel!

![default deployment](images/parallelByContainer.png)
