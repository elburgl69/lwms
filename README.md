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
