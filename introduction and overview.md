This is a working draft.


# Introduction

The echoSMs project is developing an online database for scattering model shapes and associated metadata. The purpose of this is to facilitate long-term
access to good quality scattering model input data, especially the shapes.

The aims for this hackathon are to test and get feedback on the prototype datastore, including how data are provided to it and how the data are 
retrieved from the datastore for use in scattering models.

All aspects of the prototype are up for discussion and revision - we want
the datastore to be workable for as many use cases as possible.

## The plan for today

1. Introduction to the datastore
    - Formatting shapes for loading to the datastore
      - metadata structures
      - shape data formats
    - Retrieving from the datastore
      - web API
      - use in echoSMs scattering models
1. Your hackathon server
1. Suggested workplan
    - Convert existing shape data into datastore format and validate
    - Load to your server's web API
    - Retrieve shapes from web API
    - Calculate TS from a shape
1. Feedback, summary, etc.

Ask questions, provide feedback, discuss at any time.

## Introduction to the datastore

- The datastore is an online place to store and access scattering model shapes, metadata, and raw data
- Access is via a simple RESTful web  API - documentation is [here](https://echosms-data-store-app-ogogm.ondigitalocean.app/docs)
- Intention is for to ICES to eventually host and manage it
- Input data to the datastore are TOML-formatted files containing data and metadata in a form specified by a JSON schema + unrestricted raw files
- The TOML files are validated against the schema before the datastore will accept them

### Schema overview

#### Some data structure concepts:

- A **dataset** contains data about one or more **specimens** (restricted to one species)
- Each **specimen** contains one or more **shapes**, representing parts of the specimen (e.g., body, swimbladder, backbone, etc)
- There are different shape types:
  - **outline** - dorsal and lateral outlines
  - **surface** - 3D triangular surface mesh
  - **voxels** - 3D grid of density and sound speed values
  - **categorised voxels** - 3D grid of categorised material properties

Each shape type has specific data attributes for storing the shape data (documentation on that is [here](https://ices-tools-dev.github.io/echoSMs/datastore_anatomical/#shapes)). Unit and coordinate systems are also explained at that link.

The shape format has been designed with these attributes in mind:

- easy to understand,
- everything in text files,
- general enough to store all types of shapes used in scattering models.

The schema structure is one of the main things we would like feedback on - it defines how the data are structured, what data are mandatory, their format, etc. Mike has been testing an alternative structure (schema available [here](https://github.com/ices-tools-dev/echoSMs/issues/35)). We've very interested in what is missing, what is onerous to provide, unnecessary, and improved ways of structuring the data.

Schema links:

- The schema is kept in the echoSMs [github repository](https://github.com/ices-tools-dev/echoSMs/tree/main/data_store/schema/v1). 
- The direct URL to the schema is: https://raw.githubusercontent.com/ices-tools-dev/echoSMs/refs/heads/main/data_store/schema/v1/anatomical_data_store.json
- The direct URL can be used in some online JSON schema viewers (e.g., [json-schema](https://json-schema.app/start) and [jsoncrack](https://jsoncrack.com/editor)) and validators (see below)
- A readable version of the schema is in the echoSMs [documentation](https://ices-tools-dev.github.io/echoSMs/schema/data_store_schema/).

### Preparing datasets for the datastore

In the prototype datastore, data are formatted as per the schema and then arranged in a set of directories. This is explained [here](https://ices-tools-dev.github.io/echoSMs/datastore_anatomical/#preparing-datasets-for-the-datastore).

### Validating data files

The datastore schema is formatted as a `JSON schema`. There are many existing tools that will check that a given file meets the requirements given in a JSON schema. Some online tools are:

- https://www.jsonschemavalidator.net/
- https://jsonschema.dev/
- https://mockoon.com/tools/json-schema-validator/

Some online tools can obtain the schema directly from a URL - the anatomical datastore schema URL is: https://raw.githubusercontent.com/ices-tools-dev/echoSMs/refs/heads/main/data_store/schema/v1/anatomical_data_store.json

Some Python validation packages are:

- [JSONschema](https://github.com/python-jsonschema/jsonschema)
- [jsonschema-rs](https://pypi.org/project/jsonschema-rs/)

Some R validation libraries are:

- [jsonvalidate](https://cran.r-project.org/web/packages/jsonvalidate/vignettes/jsonvalidate.html)
- [rjsoncons](https://cran.r-project.org/web/packages/rjsoncons/index.html)

A notebook that demonstrates using the Python jsonschema-rs package to validate a TOML file is available here XXXX.

### Retrieving datastore data

This is the RESTful web API. The current access methods are an initial attempt and should fit many (but not all) uses. Feedback on the usability of the access methods is welcome.

Switch to the 'XXXX' notebook to demonstrate the web API.

NOTE: the specimen and shape data returned by the API is not in the exact same structure as in the TOML files - the datastore flattens the dataset/specimen structure to make it simplier to use the data.

## Your hackathon computer

To reduce setup time we provide everyone with access to individual online Linux computers with pre-configured software and resources. You're welcome to work on your local computer if you want to but we won't have much time to help with configuration problems (the workshop files are available [here](https://github.com/gavinmacaulay/echoSMs-2026-hackathon-material), the web API code [here](https://github.com/gavinmacaulay/data_store_testing.git), and the web API setup script is [here](https://github.com/nmfs-opensci/container-images/blob/main/images/acoustics/Dockerfile) on lines 16-21).

Access to this computer is entirely via your web browser - access is via https://workshop.nmfs-openscapes.2i2c.cloud/hub/login. The password is `XXXX`.

- Choose the `Py-R - echoSMs hackathon - latest` image
- The default resource allocation is fine
- Click on `Start`
- You'll end up in a Jupyterlab instance in your web browser - it can take a few minutes to get there
- Click on this [link](https://workshop.nmfs-openscapes.2i2c.cloud/hub/user-redirect/git-pull?repo=https%3A%2F%2Fgithub.com%2Fgavinmacaulay%2FechoSMs-2026-hackathon-material&urlpath=lab%2Ftree%2FechoSMs-2026-hackathon-material%2Fintroduction%20and%20overview.md&branch=main) to download the hackathon files to your online computer
- Right click on the file that the link opened and choose `Show Markdown Preview`
- Open a Terminal (click on the new tab icon: `+`) and type these lines to start your own datastore web API:
  - `cd /data_store_testing`
  - `fastapi dev`

There are sample shape data files and Python scripts in 'YYY'.

The hackathon servers will remain there until after the April 2026 WGFAST meeting.

## Suggested work items

Some suggestions for things to try:

1. Convert your shape data (or ones we provide) into the echoSMs coordinate and unit systems
1. Generate TOML files that are suitable for loading to the web API
1. Retrieve shape data from the web API and use in a scattering model
1. Populate the metadata and give feedback
1. Use the web API and give feedback


We have provided example files on your linux server to help with the above:

- Example shape data (XXX directory)
- TOML file template
- Example code (YYY directory) to:
  - Read shape data, add metadata, validate, and write to TOML format (XXX.py)
  - Retrieve shape data and use in a scattering model (XXXX.py)

