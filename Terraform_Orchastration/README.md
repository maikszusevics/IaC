# Terraform

Terraform is an infrastructure as code tool that lets you build, change, and version cloud and on-prem resources safely and efficiently.

## What can Terraform do?
- Terraform can manage infrastructure on multiple cloud platforms.
- It's a tool for building, versioning, and delivering infrastructure as code
- Configuration files describe to Terraform the components needed to run
- Generates an execution plan describing what it will do to reach the desired state
- Executes the plan to build the described infrastructure
- Determines what changed and creates incremental execution plans
- Can manage simple components (compute instances, networking), as well as more complex components (DNS, SaaS features)


Terraform helps to simplify the process of managing reproducible infrastructure by providing two distinct types of integration: providers and modules.

- Provider: an abstraction on top of an API that makes it easy to manage resources (including importing pre-existing resources that were originally created outside of Terraform).
  
- Module: a self-contained package of Terraform configuration that provides 'best practice' implementations for which your Terraform configuration can import and consume.


## Installing Terraform
To install terraform on Windows it is reccomended to use Chocolatey installer. To setup Chocolatey, follow instructions on this [link](https://chocolatey.org/install)

Then you can run `choco install terraform` from **admin powershell** and install terraform.




## Benefits of Terraform 

- Human readable configuration language helps write infrastructure code
- Many plugins that let Terraform interact with cloud platforms via APIs
- Allows for use of resources from multiple different providers in the same resusable configurations
- Solutions for multi-cloud deployment
- Allows you to automate provisioning of infrastructure including firewall policies, databases, servers, etc.
- Can automate infrastructure deployments through existing CI/DC pipelines
- Terraform providers automatically calculate dependencies between resources to create or destroy them in the correct order

### Terraform language

