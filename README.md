# Information about Puff(ing)
At FoodX we have been releasing a lot of software, and this requires the infrastructure of our services to be maintained across many (12+) environments. We use [Azure DevOps](https://dev.azure.com) as our CI/CD; and have found that (especially) in infrastructure deployments it is best to not leverage variables in release pipelines; so we push them into git. That way we can view a single source of truth, without contamination from a secondary source.

Puff enables this, by making management of the parameters per environment land in a single yaml file. Secondly, (by design) you can review a change that could affect all environments, or changes that affect a select number of environments in a single file. Which takes cognitive load off of pull requests, that could have a disastrous effect (yes, I have 'deleted' a cosmos database in production).

Let us all agree infrastructure is only there to serve the purpose; so anything we can do to reduce the problem is valued. Making it easy, and with less error helps improve availability; and those guilty oh-sshhhh moments.

Also note worthy, is that we use [Jest](https://www.npmjs.com/package/jest) to unit test our parameters files, so that we can ensure changes are noted by people developing new services. Using JavaScript to test json, makes a lot of sense; and is the reason why puff ended up being a Node.js runtime.

# Basics
We have a couple main things that contain our services across environments:

## Name
The name is the prefix used in the file generation, it does not end up inside the template.

## Default
Values that are set in all templates through all environments and services, this is defined at the top of the template along with the name. The can be over-loaded in further definitions of the service or environment.

## Environments
Our approach is to have an environment map to an [Azure Subscription](https://docs.microsoft.com/en-us/azure/cost-management-billing/manage/create-subscription). We use subscriptions to ensure that we maintain appropriate security controls. Given that, we deploy into Dev/Test/Staging/Production Secondary/Production Primary in that sequence; to enure validity of every change we make, for infrastructure and applications.

## Regions
Region is important for us, as we deploy multiple regions per environment for active/active services; things like functions, we want to have workers running off of the [Azure Service Bus](https://azure.microsoft.com/en-us/services/service-bus/); across paired regions. Region can be specified as region (singular); or regions with a list (as multiple).

# Example
Convert yaml into Azure ARM parameters templates (json).
- On GitHub: [puff](https://github.com/Food-X-Technologies/puff).
- On NPM: [puff](https://www.npmjs.com/package/@foodx/puff).

## Get Started
### Clone
```
git clone git@github.com:Food-X-Technologies/puff-example.git
```
### NPM Install
```
npm i
```
### NPM Run Puff
Generate json parameters files, based on yaml file.
```
npm run puff
```
### NPM Delete
Delete generated files, keep your git clean!
```
npm run puffin
```

# Examples
- [Azure Functions](https://github.com/Food-X-Technologies/puff-example/tree/main/function)
- [Azure Storage Accounts](https://github.com/Food-X-Technologies/puff-example/tree/main/storage-account)

## [Input (yaml)](https://github.com/Food-X-Technologies/puff-example/blob/main/example-simple/example.yml)
```
name: ex-
key1: value1default
key2: value2default
environments:
  one:
    key3: value2one
    regions:
    - abc:
        key1: value1abc
  two:
    key4: value4two
    region: xyz
services:
  service1:
    region: abc
    key2: value2xyz
```

## Outputs (json)
### 1. [ex-service1.one.abc.json](https://github.com/Food-X-Technologies/puff-example/blob/main/example-simple/ex-service1.one.abc.json)
```
{
 "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
 "contentVersion": "1.0.0.0",
 "parameters": {
  "key1": {
   "value": "value1abc"
  },
  "key2": {
   "value": "value2default"
  },
  "region": {
   "value": "abc"
  },
  "key3": {
   "value": "value2one"
  }
 }
}
```
### 2. [ex-service1.two.xyz.json](https://github.com/Food-X-Technologies/puff-example/blob/main/example-simple/ex-service1.two.xyz.json)
```
{
 "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
 "contentVersion": "1.0.0.0",
 "parameters": {
  "key1": {
   "value": "value1default"
  },
  "key2": {
   "value": "value2default"
  },
  "region": {
   "value": "xyz"
  },
  "key4": {
   "value": "value4two"
  }
 }
}
```