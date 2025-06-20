# push tag from package.json action
> pushes a tag form the version field in the package.json

## Pre-requisites
Requires the following actions to be added to the workflow
- actions/checkout@v4
- actions/setup-node@v4

## Permissions
```yml
permissions:
  contents: write
```

## Inputs
```yml
package_json_path:
  description: 'the path to the package.json file to read the version from'
  required: false
  default: './package.json'
```

## Outputs
```yml
tag_exists:
  description: 'Indicates whether the tag exists'
  value: ${{ steps.tag.outputs.TAG_EXISTS }}
tag_name:
  description: 'The name of the tag that was pushed'
  value: ${{ steps.package.outputs.version }}
success:
  description: 'Indicates whether the tag was successfully pushed'
  value: ${{ steps.tag.outputs.TAG_EXISTS == 'false' }}
```

## Example
```yml
name: Tag Release

on:
  workflow_dispatch:
  push:
    branches: 
      - main

env:
  node_version: 22

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  
jobs:
  
  build-and-publish-tag:
    name: Create and push tag
    runs-on: ubuntu-latest

    permissions:
      contents: write

    steps:
    - uses: actions/checkout@v4
    - uses: pnpm/action-setup@v4

    - name: Use Node.js ${{ env.node_version }}
      uses: actions/setup-node@v4
      with:
        node-version: ${{ env.node_version }}

    - name: Push tag
      uses: jpbnetley/push-tag-from-package.json-action@main
     
```

## Ref
- https://github.com/jpbnetley/test-push-tag-from-package.json