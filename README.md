# Couchdb & Monk

This repository contains Monk.io template to deploy couchdb system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).


## Start

Set up Monk - https://docs.monk.io/docs/monk-in-10/

Start `monkd` and login.

```bash
monk login --email=<email> --password=<password>
```

## Clone Monk couchdb repository

In order to load templates and change configuration simply use below commands: 
```bash
git clone https://github.com/monk-io/monk-couchdb

# and change directory to the monk-couchdb template folder
cd monk-couchdb

```

## Configuration

You can add/remove configuration of the template.

The current variables can be found in `couchdb/variables` section

```yaml
  variables:
    couchdb-image-tag: "latest"  
    couchdb-user: "admin"
    couchdb-password: "password"
```

### Couchdb configuration files

You can find a configuration file in `/files` directory in repository and can edit before the running kit. There is one configuration file which bind to the container while run couchdb-monk kit 


| Configuration File	 | Format Used | Directory in Container | Purpose 
|----------|-------------|------|---------|
| **local.ini** | New style format (sysctl or ini-like)	 | ` /opt/couchdb/etc/local.d/local.ini` | Primary configuration file. Should be used for most settings. 


##  Template variables

| Variable | Description | Type | Example |
|----------|-------------|------|---------|
| **couchdb-image-tag** | Couchdb image version. | string | 3.10-management |
| **couchdb-user** | Couchdb root username. | string | admin |
| **couchdb-password** | Couchdb root user password. | string | password |


## Local Deployment

First clone the repository and change the current directory to the monk-couchdb folder and simply run below command after launching `monkd`:
:

```bash
➜  monk load MANIFEST
✨ Loaded:
 ├─🔩 Runnables:
 │  └─🧩 couchdb/couchdb
 ├─🔗 Process groups:
 │  └─🧩 couchdb/stack
 └─⚙️ Entity instances:
    └─🧩 couchdb/couchdb/metadata
✔ All templates loaded successfully

➜  monk list couchdb

✔ Got the list
Type      Template         Repository  Version  Tags
runnable  couchdb/couchdb  local       -        -
group     couchdb/stack    local       -        -

➜  monk run couchdb/stack

✔ Started local/couchdb/stack

```

This will start the entire couchdb/stack


## Cloud Deployment

To deploy the above system to your cloud provider, create a new Monk cluster and provision your instances.

```bash
➜  monk cluster new
? New cluster name couchdb
✔ Cluster created
Your cluster has been created successfully.

➜  monk cluster provider add -p gcp -f <path/to/your-key.json>
✔ Provider added successfully

➜  monk cluster grow -p gcp
? Cloud provider gcp
? Name of the new instance my-instance
? Tags (split by whitespace) couchdb
? Region europe-central2
? Zone europe-central2-a
? Instance type e2-medium
? Number of instances (or press ENTER to use default = 1) 3
? Default disk type for gcp is HDD (pd-standard). Would you like to change it? No
? Disk size (or press ENTER to use default = 250 GBs) 50
✔ Start creation of new instance(s) on gcp... DONE
✔ Creating node: my-instance-1 DONE
✔ Initializing node: my-instance-1 DONE
✔ Creating node: my-instance-2 DONE
✔ Initializing node: my-instance-2 DONE
✔ Creating node: my-instance-3 DONE
✔ Initializing node: my-instance-3 DONE
✔ Connecting: my-instance-1 DONE
✔ Syncing peer: my-instance-1 DONE
✔ Connecting: my-instance-2 DONE
✔ Connecting: my-instance-3 DONE
✔ Syncing peer: my-instance-2 DONE
✔ Syncing peer: my-instance-3 DONE
✔ Syncing nodes DONE
✔ Cluster grown successfully
```

Once cluster is ready execute the same command as for local and select your cluster (the option will appear automatically).
```bash
➜  monk load MANIFEST
✨ Loaded:
 ├─🔩 Runnables:
 │  └─🧩 couchdb/couchdb
 ├─🔗 Process groups:
 │  └─🧩 couchdb/stack
 └─⚙️ Entity instances:
    └─🧩 couchdb/couchdb/metadata
✔ All templates loaded successfully

➜  monk list couchdb

✔ Got the list
Type      Template         Repository  Version  Tags
runnable  couchdb/couchdb  local       -        -
group     couchdb/stack    local       -        -

➜  monk run couchdb/stack

✔ Started local/couchdb/stack

```

## Logs & Shell

```bash
# show Couchdb logs
➜  monk logs -l 1000 -f couchdb/couchdb

# access shell in the container running Couchdb
➜  monk shell couchdb/couchdb

```

## Stop, remove and clean up workloads and templates

```bash
➜ monk purge couchdb/stack 
✔ templates/local/couchdb/stack deleted successfully
```