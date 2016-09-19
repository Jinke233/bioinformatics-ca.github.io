---
layout: post2
permalink: /bioinformatics_on_big_data_module5_lab/
title: Bioinformatics on Big Data 2016 Module 5 Lab
header1: Bioinformatics on Big Data 2016
header2: Module 5 Lab
image: CBW_bigdata_icon.jpg
---

# Bioinformatics on Big Data Module 5 Lab

This lab was created by Solomon Shorser

## Introduction 

### Description of the lab

*write description here something like:*

Welcome to the lab for Big Data Analysis! This lab will consolidate what you have learned about Cloud Computing by using aligning reads from a cell line as an example.

After this lab, you will be able to:

* list things that they will learn

Things to know before you start:

The lab may take between 1-2 hours, depending on your familiarity with Cloud Computing and alignment tasks. Don't worry if you don't complete the lab! It will be made available for you to complete later.   
There are a few thought-provoking Questions or Notes pertaining to sections of the lab. These are optional, and may take more time, but are meant to help you better understand the visualizations you are seeing. These questions will be denoted by boxes, as follows:

   Question(s):

   * Thought-provoking question goes here
   
### Requirments

* A fresh VM
* Ubuntu 16

## Setting up your VM


### Install Java

```
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
```

### Get the dockstore tool

```
mkdir -p ~/sbin
cd ~/sbin
```

### Get the dockstore script

```
wget https://github.com/ga4gh/dockstore/releases/download/0.4-beta.4/dockstore
chomd u+x dockstore
```

### Add the location of the dockstore script to $PATH. 

You will want to add this line to the end of ~/.bashrc

```
PATH=$PATH:~/sbin
cd ~
mkdir -p ~/.dockstore
touch ~/.dockstore/config
```

#### Add to ~/.dockstore/config these lines:

##### The URL for dockstore

```
server-url: https://dockstore.org:8443
```

##### A token 

You only need a valid token if you want to push data TO dockstore. To pull data, "DUMMY" is fine.

```
token: DUMMY
```

##### Turn on caching to prevent the same input files from being downloaded again and again and again...

```
use-cache=true
```

### Install docker 

```
sudo apt-get install curl
curl -sSL https://get.docker.com/ | sh
```

Installation information can be found (here)[https://docs.docker.com/v1.8/installation/ubuntulinux/]

#### sudo groupadd docker 

This is probably not necessary - the docker installer should create this for you.   
This is so you can run `docker` without having to sudo every time.   
After you execute the line below, you will need to log out and log back in.   

```
sudo usermod -aG docker $USER
```

### Get cwltool

```
sudo apt install python-pip
pip install setuptools==24.0.3
pip install cwl-runner cwltool==1.0.20160712154127 schema-salad==1.14.20160708181155 avro==1.8.1
```

*Note:*   

If you are on **ubuntu 14**, also need `sudo pip install typing` and pip install commands will need to be run as `sudo`.   

```
sudo pip install setuptools==24.0.3 
sudo pip install cwl-runner cwltool==1.0.20160712154127 schema-salad==1.14.20160708181155 avro==1.8.1 
```


### Get the sample JSON file.

```
wget https://raw.githubusercontent.com/ICGC-TCGA-PanCancer/Seqware-BWA-Workflow/develop/Dockstore_cwl.json
```

Edit it as necessary.   
Ensure that the outputs will be saved on your local file system.   

### Fetch CWL

```
dockstore tool cwl --entry quay.io/pancancer/pcawg-bwa-mem-workflow:2.6.8-cwl1 > Dockstore.cwl
```

### Make a runtime JSON template and edit it

If you're not sure how to edit Dockstore.json, see: https://raw.githubusercontent.com/ICGC-TCGA-PanCancer/Seqware-BWA-Workflow/develop/Dockstore_cwl.json


*Note:* That examples uses S3 for output. You will probably want to store your output locally.

```
dockstore tool convert cwl2json --cwl Dockstore.cwl > Dockstore.json
```

### Run it locally with the Dockstore CLI

```
time dockstore tool launch --entry quay.io/pancancer/pcawg-bwa-mem-workflow:2.6.8-cwl1 --json Dockstore.json 
```