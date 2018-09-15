# hellobankExtractor
Téléchargez l'ensemble de vos relevés bancaire chez hello bank.

### Retrouver la liste de vos relevé au format json : ###
 * Ouvrez la page des relevés en mode développeur dans votre navigateur
 * Choisissez l'onglet "Réseaux"
 * Faite une recherche de relevé au hasard sur le site hello bank
 * Vous devriez voir le call api "https://www.hellobank.fr/demat-wspl/rest/rechercheCriteresDemat"
 * Faite clic droit sur le call et copiez AsCurl
 * Ouvrez un terminal linux et collez. Vous devriez avoir quelque chose comme ça : 
 ```
 curl 'https://www.hellobank.fr/demat-wspl/rest/rechercheCriteresDemat' -H 'Pragma: no-cache' -H 'Origin: https://www.hellobank.fr' [ ...... ] --data-binary '{"dateDebut":"07/09/2018","dateFin":"15/09/2018"}' --compressed
 ```
  * Changez les date de début et de fin pour récupérer l'ensemble de vos relevés au format json et enregistrez dans votre fichier releves.json en ajoutant à la fin de la commande `> releves.json`
 ```
 curl 'https://www.hellobank.fr/demat-wspl/rest/rechercheCriteresDemat' -H 'Pragma: no-cache' -H 'Origin: https://www.hellobank.fr' [ ...... ] --data-binary '{"dateDebut":"07/09/2010","dateFin":"15/09/2019"}' --compressed > releves.json
 ```

# Téléchargez l'ensemble des relevés
 * Vérifier que cette commande fonctionne et vous affiche l'ensemble des liens de téléchargement de vos relevés (vous devez avoir <b>jq et xarg</b>)
 ```
 cat releves.json | jq -r ".data.rechercheCriteresDemat.mapDocuments[] | .listeDocument[] | \"https://www.hellobank.fr/demat-wspl/rest/consultationDocumentDemat?ibanCrypte=\(.ibanCrypte)&idDocument=\(.idDoc)&typeCpt=\(.typeCompte)&typeDoc=RELEV&typeFamille=R001&familleDoc=\(.famDoc)&consulted=true&idLocalisation=\(.idLocalisation)&viDocDocument=\(.viDocDocument)&dateDocument=\(.dateDoc)&ikpiPersonne=\""
 ```
 * Téléchargez les relevés à l'aide de votre navigateur sur lequel vous êtes connecté à hello bank (ici chromium-browser).
 ```
 cat releves.json | jq -r ".data.rechercheCriteresDemat.mapDocuments[] | .listeDocument[] | \"https://www.hellobank.fr/demat-wspl/rest/consultationDocumentDemat?ibanCrypte=\(.ibanCrypte)&idDocument=\(.idDoc)&typeCpt=\(.typeCompte)&typeDoc=RELEV&typeFamille=R001&familleDoc=\(.famDoc)&consulted=true&idLocalisation=\(.idLocalisation)&viDocDocument=\(.viDocDocument)&dateDocument=\(.dateDoc)&ikpiPersonne=\"" | xargs -n 1 chromium-browser 
 ```
