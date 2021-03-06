# LWMS
Laravel Workflow Management System (LWMS) is an independend, configerable workflow management system that can be inlcuded in any existing Laravel project.

LWMS is independend since it installs all it's required resources to manage workflows. One can opt to bind worklflows to a model which allows you to query for the status of each workflow bound to a certain model. However the independence of the WMS os a choice. You can also opt to update model fields from workflow actions or even create your own workflow controller to manage complex workflow actions.

## What is a workflow
Within LWMS a workflow is defined as:
- A workflow is a series of predefined invokable actions that mutate the workflow statusfields from a valid begin state to a valid end state.
- A workflow has 1 or more workflow status fields to describe the workflow status. f.e. for a workflow "Order for Customers" you might have a status field for ordering the product, a status field for customer communication and a status field for delivery.
- One can define validation rules on actions. You can define what the valid result states (of each status field) for an action are. You can also define what the valid values for the status fields are for an action to be allowed to execute! You can also add callable strings to the workflow actions that are executed, return true (valid) or false (invalid) on action execution or action result.   
- Actions history is kept in the system by tracking the execution of each action with it result state.

## What does LWMS offer.
LWMS offers:
- Routes /lwms/... to query workflows statuses, workflow definition and manage the workflow.
- A router to execute the routes above...
- Abstract workflow controller as base for own workflow controllers...
- Artisan commands...
- Plugins to store Workflow definition in the file system or in a database.
- Migrations to create the workflow related tables.

## Workflow Definition.
Workflows are described in json format. Below an example workflow definition file that contains most functionality.
```JSON
  "workflows" : { 
    "workflowName1" : {
      "fields": {
        "status1" : {
          "type": "string",
          "values": ["Undefined", "Ordered", "Received", "Delayed", ],
          "undefinedVal": "Undefined",
        },
        "status2" : {
          "type": "integer",
          "values" : [0,1,2,3,],
          "undefinedVal": 0,
        },
        "externalStatus" : {
          "type" : "model",
          "values" : "\fully\qualified\namespaced\model:field"
          "undefinedVal : false,
        },
      },
      "actions" : {
        "create" : {
          "startValidation": {
            "status1" : {
              "values" : ["Undefined"],
              "callable" : "",
            },
            "status2" : {
              "values" : [],
              "callable" : "\fully\qualified\namespaced\class:method",
            },
            "externalStatus" : {
              "values": [false],
              callable: "",
            },
          },
          "resultValidation" : {
            "status1" : {
              "allowed" : true,
              "values" : ["Undefined", "Ordered"],
              "callable" : "",
            },
            "status2" : {
              "allowed" : false,
              "values" : [],
              "callable" : "\fully\qualified\namespaced\class:method",
            },
            "externalStatus" : {
              "allowed" : true,
              "values": [],
              callable: "\fully\qualified\namespaces\class:method",
            },
          },
        },
        "close" : {
          "startValidation": {
            "status1" : {
              "values" : [""],
              "callable" : "",
            },
            "status2" : {
              "values" : [],
              "callable" : "\fully\qualified\namespaced\class:method",
            },
            "externalStatus" : {
              "values": [false],
              callable: "",
            },
          },
          "resultValidation" : {
            "status1" : {
              "allowed" : true,
              "values" : ["Undefined", "Ordered"],
              "callable" : "",
            },
            "status2" : {
              "allowed" : false,
              "values" : [],
              "callable" : "\fully\qualified\namespaced\class:method",
            },
            "externalStatus" : {
              "allowed" : true,
              "values": [],
              callable: "\fully\qualified\namespaces\class:method",
            },
          },
        },
      },
      
    },
    "workflowName2": {
    
    }
  }
```
