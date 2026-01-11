This is a working draft.


# Introduction

The echoSMs project is developing an online database of scattering model shapes and associated metadata. The purpose of this is to facilitate long-term
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
1. Suggested workplan:
    - Convert existing shape data into datastore format and validate
    - Load to your server's web API
    - Retrieve shapes from web API
    - Calculate TS from a shape
1. Feedback, summary, etc.

Ask questions, provide feedback, discuss at any time.

## Introduction to the datastore

- The datastore is an online place to store and access scattering model shapes, metadata, and raw data
- Access is via simple RESTful web  API - documentation [here](https://echosms-data-store-app-ogogm.ondigitalocean.app/docs)
- Intention is for to ICES to host and manage it
- Input data to the datastore are TOML-formatted files containing data and metadata in a form specified by a schema (available [here](https://github.com/ices-tools-dev/echoSMs/blob/main/data_store/schema/v1/anatomical_data_store.json)) + unrestricted raw files
- The TOML files are validated against the schema before the datastore will accept them

### Schema overview

- A **dataset** contains data about one or more **specimens**, all of the same species
- Each **specimen** contains one or more **shapes**, representing parts of the specimen (e.g., body, swimbladder, backbone, etc)
- There are different shape types:
  - **outline** - dorsal and lateral outlines
  - **surface** - 3D triangular surface
  - **voxels** - 3D grid of density and sound speed
  - **categorised voxels** - 3D grid of categorised material properties

Each shape type has specific attributes for storing the shape data (documentation on that is [here](https://ices-tools-dev.github.io/echoSMs/datastore_anatomical/#shapes)). Unit and coordinate systems are also explained at that link.

Datasets, specimens, and shapes have mandatory and optional metadata.

A readable version of the schema documentation is [here](https://ices-tools-dev.github.io/echoSMs/schema/data_store_schema/).

The schema structure is one of the main things we would like feedback on. Mike has been testing an alternative structure (schema available [here](https://github.com/ices-tools-dev/echoSMs/issues/35)). We've very interested in what is missing, what is onerous to provide, and other ways of structuring the data.

### Retrieving datastore data

This is the RESTful web API. The current access methods are an initial go and should fit most uses. Feedback on the access methods is welcome.

Switch to the 'XXXX' notebook to demonstrate the web API.

NOTE: the specimen and shape returned by the API is not in the same structure as in the TOML files - the datastore flattens the dataset/specimen structure to make it simplier to use the data.

### Preparing datasets for the datastore

In the prototype datastore, data are formatted as per the schema and then arranged in a set of directories. This is explained [here](https://ices-tools-dev.github.io/echoSMs/datastore_anatomical/#preparing-datasets-for-the-datastore).

### Validating data files

XXXX

## Your hackathon computer

To reduce setup time we provide everyone with access to an individual online Linux computer with pre-configured software and resources. You can work
within that computer, but are welcome to work elsewhere if you wish (but we won't have much time to help with configuration problems...).

Access to this computer is entirely via your web browser - access is at https://workshop.nmfs-openscapes.2i2c.cloud/hub/login. The password is 'XXXX'.

- Choose the `Py-R - echoSMs hackathon - latest` image
- The default resource allocation is fine
- Click on `Start`
- You'll end up in a Jupyterlab instance
- Click on this [link](https://workshop.nmfs-openscapes.2i2c.cloud/hub/user-redirect/git-pull?repo=https%3A%2F%2Fgithub.com%2Fgavinmacaulay%2FechoSMs-2026-hackathon-material&urlpath=lab%2Ftree%2FechoSMs-2026-hackathon-material%2FREADME.md&branch=main) to download the hackathon files to your online computer
- Open a Terminal (click on the new tab icon (+)) and type these lines to start your own datastore web API:
  - `cd /data_store_testing`
  - `fastapi dev`

There are sample shape data files and Python scripts in 'YYY'.

Your server will remain there for a couple of weeks.
