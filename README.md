# Puff
At FoodX we have been releasing a lot of sofware, and this requires the infrastructure of our services to be maintained across many (12 and counting) environments. We use Azure DevOps as our CI/CD; and have found that (espeicially) in infrastructure deployments it is best to not leverage variables in release pipelines, and push them into git. That way we can view a single source of truth, without contamination from a secondary source.
Puff enables this, by making management of the parameters per environment land in a single yaml file. Secondly,  (by design) you can review a change that could affect all environments, or changes that affect a select number of environments in a single file. This takes cognative load off of development efforts.
Lets all aggree infrastructure is only there to serve the purpose; so anything we can do to reduce the problem is valued.

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
```
npm run puff
```

# Examples
## [Input (yaml)](https://github.com/Food-X-Technologies/puff-example/blob/main/example-simple/example.yml)
```
name: ex-
default:
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