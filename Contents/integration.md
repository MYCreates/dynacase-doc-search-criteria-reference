# Intégration avec le module docGrid {#search-criteria-ref:926b57ec-e0ef-43ad-835d-19a214dbf7a2}

Les critères sont construits de telle manière à pouvoir être simplement intégré avec le plugin docGrid.  
Pour ce faire, il suffit de modifier l'option "criterias" d'une docGrid en lui donnant le retour du getValues d'un criterias. Soit :

    [javascript]
    $("#result").on("click", function() {
        $("#myTable").docGrid("option","criterias",$("#criterias").criterias("getValues"));
    });

