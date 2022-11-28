# DevOps Capstone - Final Project

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Python 3.9](https://img.shields.io/badge/Python-3.9-green.svg)](https://shields.io/)
![Build Status](https://github.com/thelazyj/devops-capstone-project/actions/workflows/ci-build.yaml/badge.svg)
This repository contains the starter code for the project in [**IBM-CD0285EN-SkillsNetwork DevOps Capstone Project**](https://www.coursera.org/learn/devops-capstone-project?specialization=devops-and-software-engineering) which is part of the [**IBM DevOps and Software Engineering Professional Certificate**](https://www.coursera.org/professional-certificates/devops-and-software-engineering)

## Usage

You should use this template to start your DevOps Capstone project. It contains all of the code that you will need to get started.

Do Not fork this code! It is meant to be used by pressing the  <span style=color:white;background:green>**Use this Template**</span> button in GitHub. This will copy the code to your own repository with no connection back to the original repository like a fork would. This is what you want.

## Development Environment

These labs are designed to be executed in the IBM Developer Skills Network Cloud IDE with OpenShift. Please use the links provided in the Coursera Capstone project to access the lab environment.

## Useful commands

Under normal circumstances you should not have to run these commands. They are performed automatically at setup but may be useful when things go wrong:

## Project layout

The code for the microservice is contained in the `service` package. All of the test are in the `tests` folder. The code follows the **Model-View-Controller** pattern with all of the database code and business logic in the model (`models.py`), and all of the RESTful routing on the controller (`routes.py`).

```text
├── service         <- microservice package
│   ├── common/     <- common log and error handlers
│   ├── config.py   <- Flask configuration object
│   ├── models.py   <- code for the persistent model
│   └── routes.py   <- code for the REST API routes
├── setup.cfg       <- tools setup config
└── tests                       <- folder for all of the tests
    ├── factories.py            <- test factories
    ├── test_cli_commands.py    <- CLI tests
    ├── test_models.py          <- model unit tests
    └── test_routes.py          <- route unit tests
```

## Data Model

The Account model contains the following fields:

| Name | Type | Optional |
|------|------|----------|
| id | Integer| False |
| name | String(64) | False |
| email | String(64) | False |
| address | String(256) | False |
| phone_number | String(32) | True |
| date_joined | Date | False |

## Your Task

# Task 1 : Story template to be use with ZenHub .github/ISSUE_TEMPLATE/user_story_template.md

# Task 2 : Iplementing REST API's for `READ`, `UPDATE`, `DELETE`, and `LIST` while maintaining **95%** code coverage in service/routes.py and also create testing tests/test_routes.py

# Task 3 : Create Continuous integration workflow .github/workflows/ci-build.yaml

# Task 4 : Add security headers and CORS policies with Flask talisman (https = False)

# Task 5 : Countainerize microservice using Docker 'Dockerfile', create img
```docker build -t accounts .```

# Task 6 : Deploy to Kubernetes 

Create PostgreSQL database in Kubernetes for app to use
```oc new-app postgresql-ephemeral```

Create manifest def for the account deployement
```oc create deployment accounts \
  --image=us.icr.io/$SN_ICR_NAMESPACE/accounts:1 \
  --replicas=3 \
  --dry-run=client -o yaml > deploy/deployment.yaml
```

Get postgres database details
```oc describe secret postgresql```

Create deployment
```oc create -f deploy/deployment.yaml```

Create manifest def of accounts
```oc expose deploy accounts \
  --type=ClusterIP \
  --port=8080 \
  --dry-run=client -o yaml > deploy/service.yaml
```

Apply service
```oc create -f deploy/service.yaml```

Check everything running
```oc get all -l app=accounts```

Expose service using OpenShift
```oc create route edge accounts --service=accounts``` & ```oc get routes```

# Task 7 : Create CD pipeline & run

Create on OpenShift Persistent Volume Claim (PVC)
```oc create -f tekton/pvc.yaml```

Apply on OS tekton tasks
```oc apply -f tekton/tasks.yaml```

Apply on OC tekton pipeline
```oc apply -f tekton/pipeline.yaml```

Apply to my cluster
```kubectl apply -f tekton/pipeline.yaml```

Start pipeline
```tkn pipeline start cd-pipeline \
    -p repo-url="https://github.com/thelazyj/devops-capstone-project.git" \
    -p branch=main \
    -p build-image=image-registry.openshift-image-registry.svc:5000/$SN_ICR_NAMESPACE/accounts:1 \
    -w name=pipeline-workspace,claimName=pipelinerun-pvc \
    -s pipeline \
    --showlog
```

Monitor Tekton pipeline run
```tkn pipelinerun ls```

Check all deployements with the app accounts
```oc get all -l app=accounts```


## Author

[John Rofrano](https://www.coursera.org/instructor/johnrofrano), Senior Technical Staff Member, DevOps Champion, @ IBM Research, and Instructor @ Coursera

## License

Licensed under the Apache License. See [LICENSE](LICENSE)

## <h3 align="center"> © IBM Corporation 2022. All rights reserved. <h3/>
