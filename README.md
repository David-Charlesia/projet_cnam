## Enoncé du projet

Il s’agit d’une application web qui exploite des données ouvertes issues de la Bibliothèque nationale de France (BnF) disponibles sur ce portail : https://data.bnf.fr
L’adresse du “SPARQL Endpoint” : https://data.bnf.fr/sparql/ 
Deux jeux de données “au choix” :
  1) Les spectacles organisés dans différents lieux à différentes dates. Ces données peuvent être récupérées via la requête suivante :
PREFIX geo: <http://www.w3.org/2003/01/geo/wgs84_pos#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX rdagroup1elements: <http://rdvocab.info/Elements/>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX dcmitype: <http://purl.org/dc/dcmitype/>
SELECT ?Spectacle ?Titre ?Date ?focus ?Nom_Lieu ?Latitude ?Longitude
WHERE {
?Spectacle rdf:type dcmitype:Event .
?Spectacle dcterms:title ?Titre .
?Spectacle dcterms:date ?Date .
?Spectacle rdagroup1elements:placeOfProduction ?Lieu .
?Lieu foaf:focus ?focus .
?focus rdfs:label ?Nom_Lieu .
?focus geo:lat ?Latitude .
?focus geo:long ?Longitude .
} 
  2) Les documents numérisés dans Gallica (la bibliothèque numérique de la BnF) au sujet d'un lieu, avec le nom du lieu, ses coordonnées et le lien vers GeoNames (une base de données géographique) qui peuvent être extraits avec la requête suivante :
PREFIX geo: <http://www.w3.org/2003/01/geo/wgs84_pos#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
PREFIX rdarelationships: <http://rdvocab.info/RDARelationshipsWEMI/>
SELECT DISTINCT ?NumDoc ?Lieu ?Lat ?Long ?Label ?Geonames
WHERE {
?conceptLieu foaf:focus ?Lieu;
             skos:prefLabel ?Label;
             skos:exactMatch ?Geonames.
FILTER contains(str(?Geonames), "geonames")
?Lieu rdf:type geo:SpatialThing;
      geo:lat ?Lat;
      geo:long ?Long.
?conceptLieu  skos:closeMatch ?sujet.
?edition dcterms:subject ?sujet;
         rdarelationships:expressionManifested ?exp.
?exp ?s ?p.
?edition rdarelationships:electronicReproduction ?NumDoc.
}

L’application web devrait inclure les fonctionnalités suivantes :
 1) Différentes possibilités pour un internaute de rechercher une donnée en appliquant différents filtres (par exemple la date, le lieu, l’éditeur, etc.).
 2) L’affichage du résultat de recherche sous forme de tableau ou de listes, mais aussi sur un fond cartographique type Google Maps ou OpenStreetMap. Il serait possible de cliquer sur les items affichés sur le fond cartographique pour afficher davantage d’information (par exemple la description du spectacle ou du document avec, si disponible, une image d’illustration).
 3) La possibilité pour un contributeur de suggérer un ajout ou modification des données affichées. Ces suggestions doivent être stockées en local. Pour avoir le statut de contributeur, il faut s’inscrire sur le site web en tant que contributeur.
 4) La validation des suggestions d’ajouts/modifications par un expert du domaine. Pour avoir le statut d’expert, il faut s’inscrire comme expert avec une carte professionnelle issue de la BnF.
 5) La gestion, par un administrateur, des utilisateurs et des données stockées en local.
