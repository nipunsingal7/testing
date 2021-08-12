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

- `ami_name` We define the name of the ami we are creating.
- `instance_type` Here we define the type of instance we want to create. The instance family we want to use.
- `region` We define the region where we want to launch our instance.
- `source_ami_filter` It is the filter used to populate the source_ami field.
- `filters` It is the filter used to select a source_ami.
- `virtualization-type` The type of virtualization for the AMI we are building. This option is required to register HVM images.
- `name` The name of the AMI that was provided during image creation.
- `root-device-type` It is the type of root device (ie: ebs or instance-store).
- `owners` It is the list of AMI owners to limit the search. At least 1 value must be specified.
- `most_recent` If more than one result is returned, use the most recent AMI. Set it to true/false
</br>

We used sleep statement to hold the further execution of the script as apt-get update command sometimes takes more time than usual to completely execute.

```json
 "tags": {
        "Name": "packer_ami"
      }
```
</br>
We are installing tasksel, as it reduces the problem of specifying versions for php, apache and mysql.

```json
"ssh_username": "ubuntu",
      "type": "amazon-ebs"
```
</br>
We are using the lamp-server to install all the required packages, i.e mysql,php and apache. In the second command we installed the extensions needed for setting up drupal.

```json
 "provisioners": [
    {
      "type": "shell",
      "environment_vars": ["input= {{ user `packagename` }}" ],
      "scripts": ["script.sh"]

    }
   ]
```
</br></br>

## Usage

```bash
./shellscript [drupal/wordpress] [version]
```
