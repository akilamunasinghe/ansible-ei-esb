# WSO2 Enterprise Integrator Ansible scripts

This repository contains the Ansible scripts for installing and configuring WSO2 Enterprise Integrator.

## Supported Operating Systems
- Ubuntu 16.04 or higher
- CentOS 7

## Supported Ansible Versions

- Ansible 2.8.0

## Directory Structure
```
.
├── dev
│   ├── group_vars
│   │   └── ei.yml
│   ├── host_vars
│   │   └── integrator_1.yml
│   │  
│   └── inventory
├── docs
│   └── images
├── files
│   ├── lib
│   │   ├── amazon-corretto-11.0.8.10.1-linux-x64.tar.gz
│   │   └── postgresql-42.2.14.jar
│   ├── packs
│   │     └── wso2ei-6.6.0.zip
│   └── Capps
│		  └── .......
│
├── issue_template.md
├── LICENSE
├── pull_request_template.md
├── README.md
├── roles
│   │
│   ├── common
│   │   └── tasks
│   │       ├── custom.yml
│   │       └── main.yml
│   ├── integrator
│   │   ├── tasks
│   │   │   ├── custom.yml
│   │   │   └── main.yml
│   │   └── templates
│   │       ├── carbon-home
│   │       │   ├── bin
│   │       │   │   └── integrator.sh.j2
│   │       │   ├── conf
│   │       │   │   ├── axis2
│   │       │   │   │   └── axis2.xml.j2
│   │       │   │   ├── carbon.xml.j2
│   │       │   │   ├── datasources
│   │       │   │   │   └── master-datasources.xml.j2
│   │       │   │   ├── jndi.properties.j2
│   │       │   │   ├── registry.xml.j2
│   │       │   │   ├── synapse.properties.j2
│   │       │   │   ├── tomcat
│   │       │   │   │   └── catalina-server.xml.j2
│   │       │   │   └── user-mgt.xml.j2
│   │       │   └── repository
│   │       │       └── deployment
│   │       │           └── server
│   │       │               └── eventpublishers
│   │       │                   ├── MessageFlowConfigurationPublisher.xml.j2
│   │       │                   └── MessageFlowStatisticsPublisher.xml.j2
│   │       └── wso2ei-integrator.service.j2
│   │
│   └── capp_deployer
│       └── tasks
│           ├── custom.yml
│           └── main.yml
│
├── scripts
│   ├── update.sh
│   └── update_README.md
├── site.yml
└── capp_deployment.yml
```

## Files to be Copied

Copy the following files to `files/packs` directory.

1. [WSO2 Enterprise Integrator 6.6.0 package](https://wso2.com/integration/install/)

Copy the following files to `files/lib` directory.

1. [PostgreSQL Connector](https://jdbc.postgresql.org/download.html)
2. [Amazon Coretto for Linux x64 JDK](https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/downloads-list.html)

Copy the Capps to `files/Capps` , file format will be .car .


## Running WSO2 Enterprise Integrator Ansible scripts

### 1. Run the existing scripts without customization
The existing Ansible scripts contain the configurations to set-up WSO2 Enterprise Integrator [ESB profile]. In order to deploy that, you need to replace the `[ip_address]` , `[ssh_user]` and `[private_key]` given in the `inventory` file under `dev` folder by the IP of the location where you need to host the Enterprise Integrator and the SSH user. An example is given below.
```
[integrator]
wso2ei ansible_host=172.28.128.4 ansible_user=centos ansible_ssh_private_key_file=/home/centos/support-cloud
```

Run the following command to run the scripts.

`ansible-playbook -i dev site.yml`

Run the following command to deploy the Capps.

`ansible-playbook -i dev capp_deployment.yml`

If you need to alter the configurations given, please change the parameterized values in the yaml files under `group_vars` and `host_vars`.

### 2. Customize the WSO2 Ansible scripts

The templates that are used by the Ansible scripts are in j2 format in-order to enable parameterization.

#### Step 1
Uncomment the following line in `main.yml` under the role you want to customize.
```
- import_tasks: custom.yml
```

#### Step 2
Add the configurations to the `custom.yml`. A sample is given below.

```
- name: "Copy custom file"
  template:
    src: path/to/example/file/example.xml.j2
    dest: destination/example.xml.j2
  when: "(inventory_hostname in groups['sp'])"
```

Follow the steps mentioned under `docs` directory to customize/create new Ansible scripts and deploy the recommended patterns.
