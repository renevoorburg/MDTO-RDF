# MDTO-RDF

De [shacl voor de validatie van MTDO-bestanden in RDF-formaat](https://www.nationaalarchief.nl/archiveren/mdto/rdf-ontologie) zoals die gepublcieerd wordt door het Nationaal Archief, voldoet niet geheel aan de shacl-standaard. Sommige validatie-tools werken desondanks (bijvoorbeeld [shacl-validator](https://github.com/ontola/shacl-validator) of [EU ITB Shacl Validator](https://www.itb.ec.europa.eu/shacl/any/upload)) maar de shacl van het Nationaal Archief werkt helaas niet met de python module [pyshacl]](https://github.com/RDFLib/pySHACL).

De *shacl* in deze repositoryis gebaseerd op het bestand van het Nationaal Archief, maar corrigeert de fout die maakt dat **pyshacl** het niet accepteert.

## Aanpassing

De shacl van het Nationaal Archief maakt her en der de fout dat de eigenschappen van entiteiten van het type sh:PropertyShape en sh:NodeShape door elkaar gebruikt worden.
Zo bevat het originele shacl de volgende regels:

```` 
mdtosh:ObjectShape
    a sh:NodeShape ;
    sh:targetClass mdto:Object ;
    sh:name "ObjectShape" ;
    sh:description "Een fysiek, digitaal of conceptueel ding in de werkelijkheid dat van belang is voor een organisatie." ;
    sh:property mdtosh:IdentificatieShape ;
    sh:property mdtosh:NaamShape .

mdtosh:IdentificatieShape
    a sh:NodeShape ; 
    sh:path mdto:identificatie ;
    sh:class mdto:IdentificatieGegevens;
    sh:minCount 1 ;
    sh:name "Identificatie" ;
    sh:description "Gegevens waarmee het object geïdentificeerd kan worden." .
````

De gecorrigeerde shacl heeft de volgende regels:

````
mdtosh:ObjectShape
    a sh:NodeShape ;
    sh:targetClass mdto:Object ;
    sh:name "ObjectShape" ;
    sh:description "Een fysiek, digitaal of conceptueel ding in de werkelijkheid dat van belang is voor een organisatie." ;
    sh:property mdtosh:IdentificatiePropertyShape ;
    sh:property mdtosh:NaamPropertyShape .

mdtosh:IdentificatiePropertyShape
    a sh:PropertyShape ; 
    sh:path mdto:identificatie ;
    sh:minCount 1 ;
    sh:node mdtosh:IdentificatieShape .

mdtosh:IdentificatieShape
    a sh:NodeShape ; 
    sh:class mdto:IdentificatieGegevens;
    sh:name "Identificatie" ;
    sh:description "Gegevens waarmee het object geïdentificeerd kan worden." .
````



