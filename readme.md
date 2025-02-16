About Modelling and Standards Ontology (MnS)
===================
While MnS extends the foundational work of the EC3 BIM Standards Landscape Explorer by enbling the development of an OWL-based representation of the dataset, it also bridges the gap between natural language documentation of standards and machine-readable, queryable representations.  
In essence, the MnS ontology reuses other ontologies when possible. The effort was to add value by developing an ontology that (1) enables our existing relatinal dataset to be 





Motivation
==========
Despite the critical role of standards in the construction industry, their adoption is hindered by accessibility challenges, natural language complexity, and a lack of machine-readable formats. The BIM Landscape Stanadrds Explorer initiative by the M&S committee aimed to address parts of these issues/ BIM Landscape Stanadrds Explorer is connected to a carefully anlysed and curated dataset of BIM stndards and their metadata, shaped through significant over the past couple of years. However, the data was structured in a relational database on the one hand. One  other hand, the data was not machine-readble, not connected to the other datasets and not easily queryable despite being publicly available.
The MnS Ontology aims to:

* Provide a formal, semantic structure for BIM-related standards, although it can be also adapted by other domains.
* Facilitate querying and exploration of (BIM and releted) standards' metadata and relationships.
* Promote interoperability and integration of standards with other datasets.
* Establish a Link between the metadata of standards and the exisitng ontologies in the construction domain.
* In the long-term, MnS ontology aims to bridge the BIM Stnadards Landscape Explorer data and tool with the Built Environment Lookup Service (BE-OLS), which is also dveloped by the M&S committee. 

Alignment with Upper Ontologies and Domain Onotologies
==========
* Ontology Acronym: STO
* * Name: Standards Ontology
Description: Captures and connects various standards, their publishers and content. It has a good coverage of terminologies. But data properties can be sometimes difficult to use.
This is an established ontology.
Type: Upper Otnology



Ontology Acronym: SSOS
Name: The Standards Specific Ontology
Description: Defines specific aspects of standards.
It has good data properties but few classes and object properties.
This is a proposed ontology. 
Type: N/A

Ontology Acronym: DICA
Name: Digital Construction - agents
Description: Focuses on roles and responsibilities within digital construction processes and projects.
This is an established ontology.
Domain: Built Environment
Type: Domain Otnology


Ontology Acronym: OMV
Name: Ontology Metadata Vocabularies
Description: Provides metadata properties to describe and manage ontologies.
This is an established ontology. 
Type: Upper Otnology

Ontology Acronym: FOAF
Name: Friend of a Friend
Description: Models social networks, people and their activities.
This is an established ontology.
Type: Upper Otnology

Example Queries
==========
QUERY_1 (Get all standards)
    PREFIX mns: <http://example.org/ontology#>
    PREFIX sto: <https://w3id.org/i40/sto#>
    PREFIX base: <http://example.org/data#>

    SELECT ?standard ?title
    WHERE {
      ?standard a sto:standard ;
                mns:hasTitle ?title .
    }


-------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------
QUERY_2 (Get CEN standards)

    SELECT ?standard ?title
    WHERE {
    ?standard a sto:standard ;
                mns:hasTitle ?title ;
                mns:isCEN ?isCEN .
    FILTER(?isCEN = "Yes")
    }


-------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------
QUERY_3 (Get ISO standards that are not CEN)

    SELECT ?standard ?number
    WHERE {
      ?standard a sto:standard ;
                mns:hasStandardNumber ?number ;
                mns:isISO "Yes" .
      FILTER NOT EXISTS { ?standard mns:isCEN "Yes" . }
    }


-------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------
QUERY_4 (Find standards related to role)

    SELECT ?standard ?title
    WHERE {
      ?standard a sto:standard ;
                mns:hasTitle ?title ;
                mns:relatedToRole mns:Client .
    }
-------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------
QUERY_5 (Find standards related to phase)

    SELECT ?standard ?number
    WHERE {
      ?standard a sto:standard ;
                mns:hasStandardNumber ?number ;
                mns:relatedToPhase mns:Decommissioning .
    }


-------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------
QUERY_6 (Find all standards that are informatively referenced in ISO 19650-1).

    SELECT DISTINCT ?referencedStandard ?title
    WHERE {
    base:ISO_19650-1 mns:InformativelyReferences ?referencedStandard .
    OPTIONAL { ?referencedStandard mns:hasTitle ?title . }
    }


-------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------
QUERY_7 (Find standards that are essentially related to a specific topic but not related to another one at all)
    
    SELECT DISTINCT ?standard ?number ?title
    WHERE {
      ?standard a sto:standard ;
                mns:hasStandardNumber ?number ;
                mns:hasTitle ?title ;
                mns:EssentiallyRelatedToTopic mns:Data_Dictionary .
      FILTER NOT EXISTS { ?standard mns:relatedToTopic mns:Information_Container . }
    }


-------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------
QUERY_8 (Find all ontologies that are relevant to a standard related to a specific topic). 
    PREFIX mns: <http://example.org/ontology#>
    PREFIX sto: <https://w3id.org/i40/sto#>
    PREFIX omv: <http://omv.ontoware.org/2005/05/ontology#>
    PREFIX base: <http://example.org/data#>
    
    SELECT DISTINCT ?ontology ?name ?description
    WHERE {
      ?ontology a omv:Ontology ;
                omv:name ?name ;
                omv:description ?description ;
                mns:relatedToTopic mns:Data_Dictionary .
      FILTER EXISTS {
        ?standard a sto:standard ;
                  mns:relatedToTopic mns:Data_Dictionary .
      }
    }



-------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------
QUERY_9 (Find all standards in the BIM domain that are published before 2018). 

    SELECT ?standard ?number ?firstPublicationDate
    WHERE {
      ?standard a sto:standard ;
                mns:hasStandardNumber ?number ;
                mns:hasFirstPublicationDate ?firstPublicationDate ;
                sto:hasDomain base:BIM .
      FILTER (?firstPublicationDate < "2018-01-01"^^xsd:date)
}


-------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------
QUERY_10 (Which standards are normatively referenced by a standard that is related to the phase X?).

    SELECT DISTINCT ?normativelyReferencedStandard ?number
    WHERE {
    ?standard a sto:standard ;
                mns:relatedToPhase mns:Decommissioning ;
                mns:NormativelyReferences ?normativelyReferencedStandard .
    ?normativelyReferencedStandard mns:hasStandardNumber ?number.
    }

Do you have a problem? Contact us!


References
==========
BIM Standards Landscape Explorer: https://ec-3.org/BIM-Standards-Landscape-Explorer.html
