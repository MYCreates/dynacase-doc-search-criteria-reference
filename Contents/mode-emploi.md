# Mode d'emploi

## Dépendances

L'intégration du plugin requiert l'ajout dans la page web des composants suivants :

* **json2** : `lib/data/json2.js`  
    cet élément est nécessaire pour faire fonctionner le plugin sur les navigateurs ne possédant pas l'objet `JSON`.
* **jquery** : `lib/jquery/jquery.js`
* **jquery-ui** : `lib/jquery-ui/js/jquery-ui-1.8.21.custom.min.js` & `lib/jquery-ui/css/smoothness/jquery-ui-1.8.21.custom.css`
* **widget criterias** : `SEARCH_CRITERIA_UI/Layout/jquery.criterias-prod.js` & `SEARCH_CRITERIA_UI/Layout/jquery.criterias.css`

**json2** : cet élément est nécessaire pour faire fonctionner le plugin sur les navigateurs ne possédant pas l'objet JSON.

Les adresses des éléments sont données à titre indicatif et sont valable dans l'optique d'une intégration au sein d'une action/application.

*note:* Dans la version finale du produit les dépendances à criteria seront fournies dans un seul fichier js.

## Droits

Les utilisateurs devant avoir accès à Search Criteria et ses actions associées doivent avoir l'ACL : *BASIC* de l'application *SEARCH_CRITERIA_UI*.

## Initialisation

Idéalement l'initialisation du plugin doit avoir lieu sur l'évènement *ready* de la page en cours pour permettre à la DOM et aux dépendances d'être chargées.

L'initialisation de la Search Criteria se fait sur une balise `<table>`. Celle-ci doit être pré-existante à l'initialisation.
On doit ensuite la sélectionner à l'aide de jQuery et l'initialiser.

Exemple d'initialisation des criterias en utilisant une table pour les présenter sur deux colonnes :

    [html]
    <html>
        <head>
            <title>CriteriasUI</title>

            <link href="lib/jquery-ui/css/smoothness/jquery-ui.css" rel="stylesheet" type="text/css"/>
            <link href="SEARCH_CRITERIA_UI/Layout/jquery.criterias.css" rel="stylesheet" type="text/css"/>

            <script type="text/javascript" src="lib/jquery/jquery-debug.js"></script>
            <script type="text/javascript" src="lib/jquery-ui/js/jquery-ui.js"></script>
            <script type="text/javascript" src="SEARCH_CRITERIA_UI/Layout/jquery.criterias-prod.js"></script>

            <script type="text/javascript">
            $("#criterias").criterias({
                criteriasDef : {
                defaultFam : "TST_FMTCOL",
                criterias : [
                        {"id" : "title"},
                        {"id" : "tst_rellatest"},
                        {"id" : "tst_enum"},
                        {"id" : "tst_rellatests"},
                        {"id" : "tst_doubles"}
                    ]
                },
                preDefaultInsertRender : function(nb, ct) {
                    var pre = "";

                    if ((nb % 2) == 0) {
                        pre = "<tr style='width : 50%;'><td>"; 
                    }else {
                        pre = "<td>";
                    }
                    return pre;
                },
                postDefaultInsertRender : function(nb, ct) {
                    var pre = "";
                    if ((nb % 2) == 1) {
                        pre = "</td></tr>"; 
                    }else {
                        pre = "</td>";
                    }
                    return pre;
                }
            });

            $("#result").on("click", function() {
                console.log($("#criterias").criterias("getValue"));
            });
            </script>
        </head>

        <body>
        <table id="criterias" class="ui-widget" style="width : 100%; table-layout: fixed;">
        </table>
        <button id="result">Get Me Some Results</button>
        </body>

    </html>

### Options de configuration

Les éléments en gras sont obligatoires.

**criteriasDef**

:   configuration des critères  
    *type* : objet javascript,

preDefaultInsertRender
:   fonction ou chaîne de caractères qui est insérée devant chaque critère :
    
    *   fonction : la fonction prend en entrée le numéro d'insertion du critère (0 pour le 1er, 2 pour le second…) et la définition du critère,  
        et qui doit retourner une chaîne de caractères, qui sera inséré avant le critère lui-même
    *   chaîne de caractères : la chaîne est insérée avant le critère lui-même,

postDefaultInsertRender
:   fonction ou chaîne de caractères qui est insérée après chaque critère :
    
    *   fonction : la fonction prend en entrée le numéro d'insertion du critère (0 pour le 1er, 2 pour le second...) et la définition du critère,  
        et doit retourner une chaîne de caractères, qui sera inséré avant le critère lui-même,
    *   chaîne de caractères : la chaîne est insérée avant le critère lui-même,

*note :* aucune option ne peut être modifiée après la création des critères.


#### Détail de la configuration :

criteriasDef
:   cet objet de configuration donne les éléments permettant d'établir la configuration des critères.  
    Pour ce faire, il possède deux propriétés :
    
    defaultFam
    :   nom logique de la famille par défaut des critères  
        *type* :chaîne de caractères
    
    criterias
    :   tableau de définition de critères.  
        Chaque élément est un objet composé des éléments suivants :
        
        **id**
        :   nom logique de l'attribut/propriété (les propriétés utilisables sont title et state) contenu dans cette colonne  
            *type* :chaîne de caractères,
        
        famId
        :   famille contenant l'attribut id  
            *type* :chaîne de caractères,  
            Cet élément n'est pas obligatoire si defaultFam est défini ou pour le titre,
        
        label
        :   label affiché devant le critère  
            *type* :chaîne de caractères,
        
        preInsertRender
        :   fonction ou chaîne de caractères qui est insérée devant le critère (remplace preDefaultInsertRender si celui-ci est présent),
        
        postInsertRender
        :   fonction ou chaîne de caractères qui est insérée devant le critère (remplace postDefaultInsertRender si celui-ci est présent),
        
        multiplicity
        :   "simple" ou "multiple" indique si le critère est présenté sous forme pour attribut *non multiple* ou *multiple*  
            *type* :chaîne de caractères,
        
        type
        :   indique le type d'attribut ou de propriété sur lequel s'effectue la recherche  
            *type* :chaîne de caractères,
        
        urlSource
        :   url source de l'appel serveur pour les critères de type énuméré, relation, account et state.
            Cette URL doit accepter un paramètre "term" qui contient ce qui est actuellement ajouté dans le champs de recherche par l'utilisateur,  
            et retourner un objet array contenant, pour chaque ligne, un objet avec les trois propriétés key (clef stockée et non affichée), value (affiché à l'utilisateur si cet élément est sélectionné), label (présenté dans la liste déroulante)
        
        defaultValue
        :   valeur par défaut du critère. Le contenu de cet élément doit être de même type que celui du setValue.


*note :* les id de type account et color ne sont pas encore pris en compte.

## Méthodes associées

Plusieurs méthodes sont associées à l'objet Search Criteria :

destroy
:   Permet de détruire la table.  
    La table est alors supprimée (la balise reste en place, mais son contenu est vidé),

getValues
:   Permet de retrouver les valeurs des critères, cette valeur est retournée sous la forme d'un array javaScript qui contient pour chacun de ses éléments les points suivants :
    
    id
    :   identifiant du critère,
    
    kind
    :   type du critère,
    
    type
    :   type de l'attribut/propriété,
    
    multiplicity
    :   indique la multiplicité du critère (multiple ou simple),
    
    operator
    :   opérateur sélectionné,
    
    value
    :   valeur ou array de valeurs

getValue
:   Identique à `getValues` mais prend en entrée l'id d'un critère et ne retourne que la valeur de celui-ci,

setValue
:   Permet d'indiquer la valeur d'un critère. Cette fonction a comme paramètres entrant :
    
    *   id du critère
    *   valeur à donner au critère :
        
        *   chaîne de caractères pour les critères date, text, numérique,
        *   objet javascript avec les propriétés value1 et value2 pour les critères date et numérique avec l'opérateur between,
        *   objet javascript avec les propriétés value et key pour les énuméré, relation, account et state simple et tableaux d'objet javascript avec les propriétés value et key pour les énuméré, relation, account et state multiple

Ces méthodes sont appelées avec la syntaxe suivante :

     $("#criterias").criterias("fonction_name");

*note :* Suite à l'évolution de la docGrid les critères pourront lui être passée directement et ensuite être utilisés par la grid pour filtrer ses résultats.

## Évènements associés

Des évènements sont déclenchés à 2 niveaux :

*   au niveau de criteria
*   au niveau de chacun des critères


###Événements de plus haut niveau

error
:   déclenché à chaque erreur détectée.  
    Il fournit un objet event et un objet erreur (tableau de messages d'erreurs),

draw
:   déclenché après l'insertion de l'ensemble des critères de recherche

Les évènements peuvent être écoutés de deux manières différentes :

*   en utilisant les fonctionnalités pour s'attacher à un évènement de jQuery  
    Il faut alors préfixer l'évènement cible par le nom du widget en minuscule (dans notre cas *docgrid*)

    $("#criterias").on("criteriaserror", function(e, ui) { console.log(ui);});

*   en s'inscrivant directement à la création du widget :

    $("#criterias").criterias({ error : function(e, ui) { console.log(ui);}};

###Événements par critère

change
:   déclenché lorsque la valeur du critère change.  
    Le premier paramètre envoyé est un objet event, le deuxième contient la nouvelle valeur, ou la valeur ajoutée et la liste des nouvelles valeurs pour les critères enum et relation multiple

preGetValues
:   uniquement pour les critères relations et énuméré.  
    Cet évènement est déclenché avant l'envoi de la requête sur le serveur et permet d'ajouter des paramètres à la requête serveur (voir exemple ci-dessous) :

    [javascript]
    preGetValues : function(evt, ui) {
        ui.additionnalSourceParams = {
            myNewProp : 'theValue'
        };
    }

Il est possible de s'attacher aux évènements des critères lors de la création du widget critère de la manière suivante :

    [javascript]
    $("#criterias").criterias({
      criteriasDef : {
        defaultFam : "TST_FMTCOL",
        criterias : [
          {"id" : "title", 
           change : function(evt, ui) {
            console.log(ui); 
           }},
          {"id" : "tst_rellatest"},
          {"id" : "tst_enum",
           change : function(evt, ui) {
            console.log(ui); 
           }},
          {"id" : "tst_rellatests", preGetValues : function(evt, ui) {
            ui.additionnalSourceParams = {
              myNewProp : 'theValue'
            };
          }, postGetValues : function(evt, ui) {
            console.log(ui);
          },
           change : function(evt, ui) {
            console.log(ui); 
           }
          },
          {"id" : "tst_double"},
          {"id" : "tst_date"}
          ]
      }
    });
