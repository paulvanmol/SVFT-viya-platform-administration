# SAS Viya Administration Course Repository



================================================================================
FILE: README.md
================================================================================

# SAS Viya Platform Administration - Course Materials

This repository contains exercises and reference materials for the SAS Viya Platform Administration course.

## Repository Structure

- **lessons/** - Individual lesson modules organized by topic
- **scripts/** - Reusable shell scripts for common tasks
- **resources/** - Additional reference materials and examples

## Prerequisites

- Access to a SAS Viya environment
- SAS Viya CLI installed
- kubectl access to the Kubernetes cluster
- pyviyatools installed

## Getting Started

1. Clone this repository
2. Navigate to individual lesson folders for specific exercises
3. Follow the exercises in numerical order within each lesson

## Authentication Setup

Most scripts require authentication. Use the provided authentication script:

```bash
./scripts/authLoginChristine.sh
```

## Course Modules

1. Introduction to SAS Viya Administration
2. Identity Management
3. CAS Data Management
4. Security Tasks
5. SAS Programming Runtime
6. Observability

## Support

For questions about these materials, please contact your course instructor.

================================================================================
FILE: lessons/01-introduction/README.md
================================================================================

# Lesson 01: Introduction to SAS Viya Administration

## Overview

This lesson covers the fundamentals of SAS Viya administration including:
- Configuring and using SAS Viya CLI
- Setting up pyviyatools
- Using Kubernetes to examine SAS Viya
- Managing backups
- Changing configuration

## Exercises

- [01.01 - Configuring SAS Viya CLI](01-01-configure-cli.md)
- [01.02 - Setup pyviyatools](01-02-setup-pyviyatools.md)
- [01.03 - Using Kubernetes](01-03-using-kubernetes.md)
- [01.04 - Backup Management](01-04-backup-management.md)
- [01.05 - Configuration Changes](01-05-configuration-changes.md)

================================================================================
FILE: lessons/01-introduction/01-01-configure-cli.md
================================================================================

# Exercise 01.01: Configuring and Using SAS Viya CLI

## Objective

Set up and configure the SAS Viya CLI for administrative tasks.

## Prerequisites

- Terminal access (Christine Terminal)
- kubectl access to the environment

## Steps

### 1. Obtain SAS Viya CLI

The SAS Viya CLI has already been downloaded to `/usr/local/bin`.

Reference: [SAS Help Center - CLI Documentation](https://go.documentation.sas.com/doc/en/sasadmincdc/v_062/calcli/n1e2dehluji7jon1gk69yggc6i28.htm)

### 2. Set Path to Trusted CA Certificates

```bash
kubectl -n edu cp $(kubectl get pod -n edu | grep "sas-logon-app" | head -1 | awk -F" " '{print $1}'):security/trustedcerts.pem /workshop/svfcontent/trustedcerts.pem
```

### 3. Set SSL Certificate Environment Variable

```bash
export SSL_CERT_FILE=/workshop/svfcontent/trustedcerts.pem
```

### 4. Install CLI Plugins

```bash
sas-viya plugins install --repo SAS all
```

### 5. Initialize CLI Profile

Run the profile creation script:

```bash
/workshop/svfcontent/scripts/createCLIProfile.sh
```

Or manually initialize:

```bash
sas-viya profile init
```

When prompted, enter:
- **Service Endpoint**: `https://server.demo.sas.com`
- **Output type**: `json`
- **Colored output**: `n`

### 6. Authenticate

```bash
sas-viya auth login -u Christine Student1
```

## Verify Installation

Test the CLI by checking group membership:

```bash
# Get help on identities plugin
sas-viya identities --help

# List all groups
sas-viya identities list-groups

# Check SASAdministrators group membership
sas-viya identities list-members --group-id SASAdministrators
```

## Expected Results

You should see a list of members in the SASAdministrators group, confirming that the CLI is properly configured and authenticated.

================================================================================
FILE: lessons/01-introduction/01-02-setup-pyviyatools.md
================================================================================

# Exercise 01.02: Setup pyviyatools

## Objective

Install and configure pyviyatools for Python-based administration tasks.

## About pyviyatools

pyviyatools is a collection of Python scripts for administering SAS Viya. The current course image has pyviyatools pre-installed at `/opt/pyviyatools`.

Reference: [pyviyatools GitHub](https://github.com/sassoftware/pyviyatools/blob/master/INSTALL.md#viya-4)

## Verify Existing Installation

```bash
cd /opt/pyviyatools
./showsetup.py
```

## Optional: Install pyviyatools in /workshop

If you want to install pyviyatools in a different location:

### 1. Clone the Repository

```bash
cd /workshop
git clone https://github.com/sassoftware/pyviyatools.git
cd pyviyatools
```

### 2. Run Setup

```bash
./setup.py --clilocation /usr/local/bin --cliexecutable sas-viya
```

### 3. Install Python Requirements

```bash
pip install -r requirements.txt
```

## Test pyviyatools

### 1. Authenticate to Viya

```bash
sas-viya profile init
```

Enter:
- Service endpoint: `https://server.demo.sas.com`
- Output format: `json`
- Colored output: `n`

```bash
sas-viya auth login
```

Enter credentials:
- Username: `christine`
- Password: `Student1`

### 2. Execute Test Call

```bash
./callrestapi.py -e /folders/folders -m get
```

This should return JSON data for folders in the Viya deployment.

### 3. Verify Setup

```bash
./showsetup.py
```

This displays:
- Python and package versions
- Environment variable settings
- Profile information
- Current user

## Example: Check Folder Authorizations

```bash
python explainaccess.py -f "/Orion Star/Marketing" --header > /workshop/svfcontent/marketing_effectiveaccess.csv
```

This exports the effective access permissions for the Marketing folder to a CSV file.

================================================================================
FILE: scripts/authLoginChristine.sh
================================================================================

#!/bin/bash
# Authentication script for Christine user

sas-viya auth login -u Christine Student1

================================================================================
FILE: scripts/addMarketingGroup.sh
================================================================================

#!/bin/bash
# Create Marketing group and add members

# Source authentication
source /workshop/scripts/authLoginChristine.sh

# Create Marketing group
sas-viya identities create-group --name Marketing --id marketing

# Add members
sas-viya identities add-member --group-id marketing --user-member-id Abbott
sas-viya identities add-member --group-id marketing --user-member-id christine
sas-viya identities add-member --group-id marketing --user-member-id Douglas
sas-viya identities add-member --group-id marketing --user-member-id Delilah

echo "Marketing group created successfully with 4 members"

================================================================================
FILE: lessons/02-identity-management/README.md
================================================================================

# Lesson 02: Identity Management

## Overview

This lesson covers identity management in SAS Viya including:
- Configuring LDAP filters
- Managing custom groups
- Creating authentication domains
- Managing external credentials

## Exercises

- [02.01 - LDAP Group Filtering](02-01-ldap-filtering.md)
- [02.02 - Create Custom Groups](02-02-custom-groups.md)
- [02.03 - CAS Host Account Groups](02-03-cas-host-groups.md)
- [02.04 - Managing External Credentials](02-04-external-credentials.md)

================================================================================
FILE: lessons/03-cas-data-management/README.md
================================================================================

# Lesson 03: CAS Data Management

## Overview

This lesson covers CAS data management including:
- Creating caslibs
- Importing data
- Managing table state
- Working with format libraries

## Exercises

- [03.01 - Create Workshop Caslib](03-01-workshop-caslib.md)
- [03.02 - Import Local Files](03-02-import-files.md)
- [03.03 - CAS Table State Management](03-03-table-state.md)
- [03.04 - Working with Format Libraries](03-04-format-libraries.md)

================================================================================
FILE: scripts/addWorkshopCaslib.sh
================================================================================

#!/bin/bash
# Create Workshop caslib pointing to course data

source /workshop/scripts/authLoginChristine.sh

sas-viya cas caslibs create path \
  --path /workshop/svfcontent/data \
  --name Workshop \
  --description "Workshop data for class" \
  --server cas-shared-default

echo "Workshop caslib created successfully"

================================================================================
FILE: scripts/loadWorkshopTables.sh
================================================================================

#!/bin/bash
# Load all tables in Workshop caslib to memory

source /workshop/scripts/authLoginChristine.sh

sas-viya cas tables load \
  --caslib Workshop \
  --table=* \
  --server cas-shared-default

echo "Workshop tables loaded to CAS memory"

================================================================================
FILE: lessons/04-security/README.md
================================================================================

# Lesson 04: Security Tasks

## Overview

This lesson covers security configuration including:
- Managing folder access
- Securing caslibs
- Row-level security
- Authorization rules

## Exercises

- [04.01 - Create Folder Structure](04-01-folder-structure.md)
- [04.02 - Access to Functionality](04-02-functionality-access.md)
- [04.03 - Secure Finance Folder](04-03-secure-finance.md)
- [04.04 - Secure Finance Caslib](04-04-secure-caslib.md)

================================================================================
FILE: resources/cas-format-management.sas
================================================================================

/* CAS Format Management Example */
/* Ensure you have an active CAS session */
cas myCAS;

/* 1) Create (or reference) a path-based global caslib for formats */
proc cas;
  session casauto;
  
  /* Create a caslib if it doesn't already exist */
  /* Replace /shared/formats with your shared path */
  action addCaslib /
    name="formats"
    path="/workshop/svfcontent/SASFormats"
    datasource={srctype="path"}
    /* promote=TRUE makes it global (visible to all sessions/users) */
    promote=TRUE;
  
  /* 2) Create a CAS format library (backed by a .sashdat) */
  action formatManagement.createFormatLibrary /
    formatLibName="svfformats"
    caslib="formats";
  
  /* 3) Import formats from a SAS 9.4 catalog (UTF-8) */
  action formatManagement.importSASFormats /
    formatLibName="svfformats"
    source={
      type="catalog",
      path="/workshop/svfcontent/SASFormats/formats.sas7bcat"
    };
  
  /* 4) Append the format library to CAS search order */
  action formatManagement.updateSearchOrder /
    operation="append"
    formatLibNames={"svfformats"};
quit;

/* Optional: verify search order */
proc cas;
  action formatManagement.listSearchOrder;
  action formatManagement.listFormatLibraries;
quit;

================================================================================
FILE: .gitlab-ci.yml
================================================================================

# GitLab CI/CD configuration (optional)
# This file can be used to set up automated testing or deployment

stages:
  - validate
  - test

validate_scripts:
  stage: validate
  script:
    - echo "Validating shell scripts..."
    - find scripts -name "*.sh" -exec bash -n {} \;
  only:
    - main
    - merge_requests

validate_markdown:
  stage: validate
  script:
    - echo "Validating markdown files..."
    - find . -name "*.md" -exec echo "Checking {}" \;
  only:
    - main
    - merge_requests
