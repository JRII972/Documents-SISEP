Title: Principe de fonctionnement
Summary: A brief description of my document.
Authors:
    - Jérémy JOVINAC
Date: 2025-04-25
some_url: https://selenium-python.readthedocs.io/

# Introduction au scrapping

Le web scraping est une technique d'extraction du contenu de sites Web, via un script ou un programme, dans le but de le transformer pour permettre son utilisation dans un autre contexte comme l'enrichissement de bases de données, le référencement ou l'exploration de données. C'est ce que font [les scripts CMS (web)](Enquêtte/Prix et marché/CMS (Web)/Scripts/index.md).

La recherche automatiser des prix sur les sites internes ou API des CMS repose principalement sur un outil de test automatiser : [**Selenium**](https://selenium-python.readthedocs.io/ "Ceci est une documentation *non-officiel* pour python") :material-information-outline:{ title="Il existe une version utilisable sous R, JAVA et d'autre langage commun. La structure du code sera sensiblement similaire" }


Le script va accéder au site, recherche les bares de recherche et bouton par XPath, CSS ou ID; et selon les sites va récupérer aux emplacements donnés le contenue des balises HTML ou leurs attributs.

```html
<div id="ID_de_la_Balise" class="Classe_CSS" data-value="attribut">
    <p>Contenue HTML de la balise p</p>
<div>
```

??? note "XPath et CSS Selector"
    XPath peut être utilisé pour naviguer dans les éléments et les attributs d’un document XML.
    ```
    'div[@id="ID_de_la_Balise"]/p'
    ```

    Le **CSS Selector** est similaire à ce dernier, mais au lieu de donner le chemin, il donne un paterne. Et l'interpréteur cherchera celui-ci.
    ```
    '[class*="PrixUnitairePartieDecimale"] > p' 
    ```
    Celui-ci cible tout paragraphe, enfant de n'importe quel éléments html :material-information-outline:{ title="Le * est sous entendu"}, ayant une de leurs classes qui contient *'PrixUnitairePartieDecimale'*. 

    Ils utilisent les mêmes syntaxes pour les sélecteurs. [Documentation](https://devhints.io/xpath)

## Site avec une génération coté utilisateur
Certain site, pour une meilleur fluidité, interactivité ou même sécurité n'envoie pas directement une page HTML avec le contenue tel que vous le voyer. Beaucoup on des pages quasiment vide, avec juste un code de génération. Ce dernier vas construire la structure de la page, puis la remplir en faisant différents appel au serveur.

Cela à un impact sur la méthode, on ne peux pas simplement télécharger le contenue de la première requête et la parser :material-information-outline:{ title="Analyser le contenue à la recherche d'information ou de contenue structurer"}. Il faut simuler le navigateur, exécuter les scripts (*en laissant passer les requêtes AJAX*), puis parser. Cela demande donc d'attendre le chargement complet de la page (*ou du moins du contenue rechercher*). 

Pour accomplir cela, sur certain élément clé, on crée une boucle qui vas attendre que l’élément soit :

    - Clickable
    - Visible
    - Accessible
    - Change d'état
    - Ou simplement existe



!!! note "Exemple de code"
    ``` py
    from selenium.common.exceptions import NoSuchElementException
    from selenium.common.exceptions import StaleElementReferenceException
    from selenium.webdriver.common.by import By
    from selenium.webdriver.support.ui import WebDriverWait
    from selenium.webdriver.support import expected_conditions

    ignored_exceptions=(NoSuchElementException,StaleElementReferenceException,) # (1)

    listProduits = WebDriverWait( driver, 15, ignored_exceptions=ignored_exceptions) \
            .until(expected_conditions.presence_of_element_located((By.ID, listProduitsID))) \
                .find_elements(By.CSS_SELECTOR, 'li[class*="Product"]')  # (2)
    ```

    1.  Exception à ne pas relever
    2.  Dans le script Leclerc, permet d'attendre que tout les produits soient bien chargé
    


??? warning "Boucle infini"
    Un timeout doit être donner de manière raisonnable en fonction du débit réseau, de façon à éviter des arrêt prématurés, ou un chargement qui donne l'impression de ne pas s'arrêter. 

    Afficher un message pour l'utilisateur via le terminal ou via une boite de dialogue JS dans ces cas, pour éviter un arrêt prématurée par un utilisateur.

## Proxy
Pour fonctionner ces scripts on besoin d'un accès à internet (extérieur). Hors, au sein de la DAAF, pour sécurisé le réseau, un proxy avec authentification est installer. Mais ce dernier est actuellement configurer sur Firefox seulement. Si vous exécuter ces scripts :material-information-outline:{ title="Même en utilisant les drivers de firefox"} il n'utilisera pas cette configuration. De plus, le mettre dans la configuration windows ne suffira pas dans certain cas. Il ne *faut pas* qu'il y ai de demande d'authentification durant l’exécution du script, sinon celui-ci renverra une erreur.
Pour contourner tout cela, les scripts vont crée un profile utilisateur pour l'instance de test, et lui donner des identifiants de connexion.

!!! warning "Configuration via windows"
    Ajouter la configuration via windows va permette à tout les applications qui le demande d'avoir la configuration proxy. Malheureusement, celle actuelle est un script d’installation. Ce qui veux dire que ce dernier doit être résolue par l'application pour connaître la bonne adresse du proxy (*rie.proxy.national.agri:8080*). Et enfin vous demander le mot de passe. Cette dernière opération n'est pas gérer par le script.

    Pour ajouter la configuration via windows allez dans les paramètres : **Réseau et Internet** > **Proxy** et ajouter votre proxy.
    Dans l'absolue c'est une configuration manuelle qui serait optimal : 
    ```
    protocole://pseudo:mot_de_passe@adresse.proxy:port
    ```

!!! danger "Règle de sécurité"
    Ne connecter pas votre poste **en même temps** à un réseau non sécurisé *ex : partage de connexion avec un téléphone* à celui de la DAAF. 
    Cela pourrais ouvrir une faille de sécurité.

