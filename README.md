# python-gcf
A test repository to build a python package and publish the package to Artifact Registry using GCB. Then have the package be a dependency in a GCF function.

## Pushing a package to Artifact Registry using GCB
### sample
The package to be pushed is in the `sample` folder. It contains a simple `__init__.py` that contains a greeting.

### setup.py
The setup.py outlines the necessary information for the package to be built so that it can be published. 

### requirements.txt
This file contains the necessary tools and packages that have to be installed. 
  - `twine` is the python tool that uploads packages.
  - `build` is the python tool to build distributions for your package. 
  - `keyrings.google-artifactregistry-auth` is our keyring backend that works to authenticate with AR repositories.

### cloudbuild.yaml
This file outlines the steps that cloudbuild will run. 
1. The first step is to install each of the packages (from the central PyPI repository) that are listed in the `requirements.txt`.
2. Then we will build the package using the information that is in the `setup.py` file.
3. Finally we use twine to upload the package to the specified AR repository. 

The command to run cloudbuild is `gcloud builds submit --config cloudbuild.yaml`.

## Configure the function to use the package as a dependency
### python-sample-function
The python-sample-function folder contains the function that will be deployed to cloud functions. 

### main.py
This is the file that defines your function. In this example we are going to be using the package that we uploaded and use it as an import. Our function should return the greeting that we had uploaded to AR. 

### requirements.txt
The `extra-index-url` tells GCF to install the dependency from the specified repository.


The command to deploy the function is: 
`gcloud functions deploy hello_team --runtime python39 --trigger-http --allow-unauthenticated`. Where hello_team is the function that was defined in the `main.py`.

The command to test the function is: 
`gcloud functions call hello_team`. Where it should return result `Hello Team`.
