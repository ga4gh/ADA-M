# ADA-M

The Automatable Discovery and Access Matrix (ADA-M) provides a standardized way to unambiguously represent the conditions related to data discovery and access. By adopting ADA-M, data custodians can generally describe what their data are (the Header section), who can access them (the Permissions section), terms related to their use (the Terms section), and special conditions (the Meta-Conditions). By doing so, data custodians can participate in data sharing and collaboration by making meta information about their data computer-readable and hence directly available for digital communication, searching and automation activities.

The ADA-M standard began development in late 2015 with the participation of over 50 volunteers form academia and industry and the first version (version 1.0) [published in late 2016](https://genomicsandhealth.org/work-products-demonstration-projects/automatable-discovery-and-access-matrix).

## OpenAPI

This repository has recently been ported from Protocol Buffer to the OpenAPI format in an effort to consolidate all GA4GH software to a common standard.

Below are specifications and notes based on the Protocol Buffer version. We will be updating this documentation to reflect he OpenAPI standard shortly.

## Protocol Buffer (deprecated)

This repository is focussed on providing an API implementation of ADA-M version 1.0 using [Protocol Buffer](https://developers.google.com/protocol-buffers/) for easy serialization of an ADA-M data description. Protocol Buffer makes it relatively easy to compile the .proto to your language of choice and facilitate the adoption of the ADA-M standard for collaboration in health and genomic data exchange.

While ADA-M .proto file can be compiled to multiple languages, we currently provide the Python compiled version for demonstration purposes.

## Overview of the ADA-M meta description sections

ADA-M aims to help data custodians assign a discovery and access 'Profile' to their dataset by stipulating formally and in a machine-readable way the conditions for access to these data. The following describes the four sections that comprise the fields according to ADA-M version 1.

### Header Section

The Header section is used to generally describe the source of the data and what they are. The Protocol Buffer .proto description of ADA-M is defined according to the following 11 Header section fields:

- `string matrixName` the custom name of the matrix                             
- `string matrixVersion` the version of the matrix e.g., "1.0"
- `repeated string matrixReferences` to state any reference to this matrix e.g., publications, URLs, DOIs for the matrix (there can be many).
- `string matrixProfileCreateDate` the creation date of this matrix.
- `repeated ProfileUpdates matrixProfileUpdates` the date and description of any updates made to the fields of this matrix (there can be many).
- `string resourceName` the name of the data resource this matrix relates to.
- `repeated string resourceReferences` to state any existing reference to the data resource e.g., publications, URLs, DOIs for the resource (there can be many).
- `string resourceDescription` a description of the data resource.
- `DataLevel.Level resourceDataLevel` the type of level of the data resource (either UNKNOWN, DATABASE, METADATA, SUMMARISED, DATASET, RECORDSET, RECORD, or RECORDFIELD)
- `repeated Contact resourceContactNames`  the name and email of the contacts (there can be many) for the data resource.
- `repeated string resourceContactOrganisations` the name of the organisation that is the custodian of the data resource.

### Profile Section

The Profile section is used to describe the individual or organisation profile for the data requestor. 

- `RestrictionsUL.Restrictions country`
- `repeated Description allowedCountries`

- `RestrictionsUL.Restrictions organisation`
- `repeated Description allowedOrganisations`

- `RestrictionsULF.Restrictions nonProfitOrganisation`
- `repeated Description allowedNonProfitOrganisations`

- `RestrictionsULF.Restrictions profitOrganisation`
- `repeated Description allowedProfitOrganisations`

- `RestrictionsUL.Restrictions person`
- `repeated Description allowedPersons`

- `RestrictionsULF.Restrictions academicProfessional`
- `repeated Description allowedAcademicProfessionals`

- `RestrictionsULF.Restrictions clinicalProfessional`
- `repeated Description allowedClinicalProfessionals`

- `RestrictionsULF.Restrictions profitProfessional`
- `repeated Description allowedProfitProfessionals`

- `RestrictionsULF.Restrictions nonProfessional`
- `repeated Description allowedNonProfessionals`

- `RestrictionsULF nonProfitPurpose`
- `repeated Description allowedNonProfitPurposes`

- `RestrictionsULF profitPurpose`
- `repeated Description allowedProfitPurposes`

- `RestrictionsULF.Restrictions researchPurpose`
- `repeated Description allowedResearchPurposes`
- `repeated ResearchDescription allowedResearchProfiles`

- `RestrictionsULF.Restrictions clinicalPurpose`
- `repeated Description allowedClinicalPurpose`
- `repeated ClinicalDescription allowedClinicalProfiles`

### Terms Section

The Term section is used to describe the terms or conditions under which a data requestor must abide to with respect to the use of the data.

- `bool noAuthorizationTerms`
- `repeated string whichAuthorizationTerms`

- `bool noPublicationTerms`
- `repeated string whichPublicationTerms`

- `bool noTimelineTerms`
- `repeated string whichTimelineTerms`

- `bool noSecurityTerms`
- `repeated string whichSecurityTerms`

- `bool noExpungingTerms`
- `repeated string whichExpungingTerms`

- `bool noLinkingTerms`
- `repeated string whichLinkingTerms`

- `bool noRecontactTerms`
- `repeated string allowedRecontactTerms`
- `repeated string compulsoryRecontactTerms`
  
- `bool noIPClaimTerms`
- `repeated string whichIPClaimTerms`
  
- `bool noReportingTerms`
- `repeated string whichReportingTerms`

- `bool noCollaborationTerms`
- `repeated string whichCollaborationTerms`

- `bool noPaymentTerms`
- `repeated string whichPaymentTerms`

### Meta-Conditions Section

The Meta-Conditions section is used to describe aspects of the data that have to do with how the matrix should be used and othe special conditions.

- `SharingMode sharingMode`
- `MultipleObligationsRule multipleObligationsRule` the intepretation rule if multiple obligatory profiles are specified.
- `bool noOtherConditions` to state if there are no other use restrictions/limitations in force which are not herein specified.
- `repeated string whichOtherConditions` other permissions/limitations may apply as specified.
- `bool sensitivePopulations` to state if there is a special evaluation required for access requests involving sensitive/restricted populations.
- `bool uniformConsent` to state if identical consent permissions have been provided by all subjects

## Installation using Python

We recommend setting up a virtual environment using virtualenv to contain all your python packages related to your project in their own environment.

Install globally with pip:
`> pip install virtualenv`

Move to your preferred directory:
`> cd MyDirectory`

Create the virtual environment:
`> virtualenv .myproject-env`

Activate your virtual environment:
`> source .myproject-env/bin/activate`

Now install protobuf
`(.myproject-env) > pip install protobuf`

Now git clone or download ADAM.py to this directory and you can get started.

## Example Usage

```
from ADAM import Header, Profile, Terms, MetaConditions

header = Header()
header.matrixName = 'My First ADA-M Matrix'

```

Now let's save it by first serializing it with SerializeToString():

```
string_representation = header.SerializeToString()

f = open('My_First_ADAM_Matrix','w')
f.write(string_representation)
f.close()

```

Now let's open it up again in a new header using ParseFromString(data):

```
new_header = Header()

with open('My_First_ADAM_Matrix', 'r') as f:
    new_header.ParseFromString(f.read())

print new_header.matrixName
```

Will output 'My Frist ADA-M Matrix'

For more examples using Protocol Buffer with Python please [read the tutorial](https://developers.google.com/protocol-buffers/docs/pythontutorial).

