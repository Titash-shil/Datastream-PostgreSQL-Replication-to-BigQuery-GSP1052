# Datastream: PostgreSQL Replication to BigQuery || [GSP1052](https://www.cloudskillsboost.google/focuses/53925?parent=catalog) ||

# # Like, comment, share & Don't forget to subscribe [Qwiklab_Explorers_ts](https://youtube.com/@titashshil?si=RgamNu1dc9jVIbJN) 👍😄🤝

### Run the following Commands in CloudShell

```
export 
```
```
export ZONE=$(gcloud compute project-info describe \
--format="value(commonInstanceMetadata.items[google-compute-default-zone])")

export REGION=$(echo "$ZONE" | cut -d '-' -f 1-2)

gcloud services enable sqladmin.googleapis.com
gcloud services enable datastream.googleapis.com

POSTGRES_INSTANCE=postgres-db
gcloud sql instances create ${POSTGRES_INSTANCE} \
    --database-version=POSTGRES_14 \
    --cpu=2 --memory=10GB \
    --authorized-networks=${DATASTREAM_IPS} \
    --region=$REGION \
    --root-password pwd \
    --database-flags=cloudsql.logical_decoding=on

gcloud sql connect postgres-db --user=postgres
```

* When prompted for the password, paste the following.

```
pwd
```
```
CREATE SCHEMA IF NOT EXISTS test;

CREATE TABLE IF NOT EXISTS test.example_table (
id  SERIAL PRIMARY KEY,
text_col VARCHAR(50),
int_col INT,
date_col TIMESTAMP
);

ALTER TABLE test.example_table REPLICA IDENTITY DEFAULT; 

INSERT INTO test.example_table (text_col, int_col, date_col) VALUES
('hello', 0, '2020-01-01 00:00:00'),
('goodbye', 1, NULL),
('name', -987, NOW()),
('other', 2786, '2021-01-01 00:00:00');

CREATE PUBLICATION test_publication FOR ALL TABLES;
ALTER USER POSTGRES WITH REPLICATION;
SELECT PG_CREATE_LOGICAL_REPLICATION_SLOT('test_replication', 'pgoutput');
```
```
exit
```
```
#!/bin/bash
# Define color variables

BLACK=`tput setaf 0`
RED=`tput setaf 1`
GREEN=`tput setaf 2`
YELLOW=`tput setaf 3`
BLUE=`tput setaf 4`
MAGENTA=`tput setaf 5`
CYAN=`tput setaf 6`
WHITE=`tput setaf 7`

BG_BLACK=`tput setab 0`
BG_RED=`tput setab 1`
BG_GREEN=`tput setab 2`
BG_YELLOW=`tput setab 3`
BG_BLUE=`tput setab 4`
BG_MAGENTA=`tput setab 5`
BG_CYAN=`tput setab 6`
BG_WHITE=`tput setab 7`

BOLD=`tput bold`
RESET=`tput sgr0`
#----------------------------------------------------start--------------------------------------------------#

echo "${BG_MAGENTA}${BOLD}Starting Execution${RESET}"

POSTGRES_INSTANCE=postgres-db
export PUBLIC_IP=$(gcloud sql instances describe $POSTGRES_INSTANCE --format="value(ipAddresses[0].ipAddress)")
export ZONE=$(gcloud compute project-info describe \
--format="value(commonInstanceMetadata.items[google-compute-default-zone])")
export REGION=$(echo "$ZONE" | cut -d '-' -f 1-2)
echo "${CYAN}${BOLD}Click here: "${RESET}""${BLUE}${BOLD}""https://console.cloud.google.com/datastream/connection-profiles/create/POSTGRESQL?project=$DEVSHELL_PROJECT_ID"""${RESET}"
echo "${YELLOW}${BOLD}your REGION is${RESET}" "${GREEN}${BOLD}"$REGION"${RESET}"
echo "${RED}${BOLD}Copy this: "${RESET}""${WHITE}${BOLD}"$PUBLIC_IP""${RESET}"

#-----------------------------------------------------end----------------------------------------------------------#
```

* Use **postgres-cp** as the name and ID of the connection profile.

| Field | Value |
| :---: | :----: |
| Username | **postgres** |
| Password  | **pwd** |
| Database | **postgres** |

* Use **bigquery-cp** as the name and ID of the connection profile.

* Use **test-stream** as the name and ID of the stream

* Specify the replication slot name as **test_replication**

* Specify the publication name as **test_publication**
  

# Congratulations ..!!🎉  You completed the lab shortly..😃💯

# *Well done..!* 👏

# Thank you for visiting.... :) 🗯️

# [Qwiklab_Explorers_ts](https://youtube.com/@titashshil?si=RgamNu1dc9jVIbJN)
