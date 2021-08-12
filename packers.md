# Packers
</br>

## Table of Contents
- [Description](#Description)
- [Features](#Features)
- [Code Explanation](#Code-Explanation)
- [Usage](#Usage)
</br></br>

## Description
This is the packer code for creating an AMI. An instance will be launched and all the required dependencies will be put in the instance and then the AMI will be created. Once the AMI is created the instance will be automatically terminated. This will make an AMI with all the required dependencies before hand.
</br></br>
## Features
- 
</br></br>

## Code Explanation


In this code we are creating the AMI by defining its name and instnce type we will be launching and various other parameters. 

```json
      "ami_name": "ion-packer-{{timestamp}}",
      "instance_type": "t2.micro",
      "region":"us-east-1",
      "source_ami_filter": {
        "filters": {
          "virtualization-type": "hvm",
          "name": "ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-*",
          "root-device-type": "ebs"
        },
        "owners": ["099720109477"],
        "most_recent": true
      }
```

- `ami_name` We define the name of the ami we are creating. Timestamp will put the time at which AMI is created.
- `instance_type` Here we define the type of instance we want to create. The instance family we want to use.
- `region` We define the region where we want to launch our instance.
- `source_ami_filter` It is the filter used to populate the source_ami field.
- `filters` It is the filter used to select a source_ami.
- `virtualization-type` The type of virtualization for the AMI we are building. This option is required to register HVM images.
- `name` The name of the AMI that was provided during image creation.
- `root-device-type` It is the type of root device (ie: ebs or instance-store).
- `owners` It is the list of AMI owners to limit the search. At least 1 value must be specified.
- `most_recent` If more than one result is returned, use the most recent AMI. Set it to true/false
</br></br>
This piece of code defines the tag name for our AMI that we will be creating. Tag names makes it easier to find our AMI's. 

```json
 "tags": {
        "Name": "packer_ami"
      }
```
</br></br>
This code defines the configuration for communication.

```json
"ssh_username": "ubuntu",
      "type": "amazon-ebs"
```
- `ssh_username` We define the username to connect to SSH with. Required if using SSH.
</br></br>
Provisioners use builtin and third-party software to install and configure the machine image after booting. Provisioners prepare the system for use, common use cases for provisioners include: installing packages, patching the kernel, creating users and downloading application code.
Here we are defining the name of our shell script which will run after booting this AMI. This shell script will contain code to install various packages in the system.

```json
 "provisioners": [
    {
      "type": "shell",
      "environment_vars": ["input= {{ user `packagename` }}" ],
      "scripts": ["script.sh"]

    }
   ]
```
- `type` We define the type of provisioner to use.
- `environment_vars` Here we define the environment variables, the inputs shell script requires to execute.
- `scripts` "shell" provisioner requires the path of the script, so we define it here. 
</br></br>

## Usage

```bash
./shellscript [drupal/wordpress] [version]
```
