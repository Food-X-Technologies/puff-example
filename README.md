# Puff Example
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