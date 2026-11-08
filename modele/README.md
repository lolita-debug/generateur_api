# Modèle Power BI : `Modele_API_Melodi.pbit`

Un modèle Power BI qui contient déjà les 7 requêtes du générateur, avec leurs descriptions. Il évite de les coller à la main dans chaque nouveau rapport.

| | |
|---|---|
| Fonctions | `fn_MelodiDico` v1.2, `fn_MelodiData` v1.5 (identiques à l'onglet « Fonctions » du générateur) |
| Paramètres demandés à l'ouverture | `P_Millesime_RP` (millésime du recensement, `2023` par défaut), `P_Departements_Perimetre` (`67,68` par défaut ; `2A,2B` pour la Corse ; vide pour la France entière, déconseillé) et `P_Chemin_Referentiel` (adresse de votre copie du référentiel : **la valeur proposée est un exemple fictif, à remplacer**) |
| Tables chargées | `DIM_Referentiel_Geographique` (référentiel des communes) et une requête d'exemple, `FAIT_RP_Population_AgeDetaille_Alsace`, reliée au référentiel |
| Ce qu'il ne contient pas | Aucune donnée (un .pbit n'enregistre que la structure), aucun thème, aucun identifiant de connexion |

## Utilisation

1. **Installer le référentiel** (une fois) : télécharger [`referentiel/Referentiel_Geographique_2026.xlsx`](../referentiel/Referentiel_Geographique_2026.xlsx), le déposer sur SharePoint ou OneDrive, et copier son lien direct (Excel, application de bureau : Fichier › Informations › Copier le chemin d'accès, sans la fin `?web=1`). Pour un simple essai : `https://raw.githubusercontent.com/lolita-debug/generateur_api/main/referentiel/Referentiel_Geographique_2026.xlsx`.
2. Télécharger `Modele_API_Melodi.pbit` et l'ouvrir avec Power BI Desktop (double-clic).
3. Renseigner les paramètres demandés, dont `P_Chemin_Referentiel` avec le lien de l'étape 1, puis cliquer sur **Charger**.
4. À la première connexion :

   | Source | Identifiants | Confidentialité |
   |---|---|---|
   | `api.insee.fr` | Anonyme | Le même niveau que le référentiel |
   | Référentiel sur SharePoint ou OneDrive | Compte d'organisation | Organisationnel |
   | Référentiel sur GitHub (essai) | Anonyme | Public |

5. Ajouter ses propres requêtes avec le générateur. L'exemple peut être supprimé.

---

## Pour la personne qui maintient le dépôt : créer ou mettre à jour le modèle

Le modèle est construit à la main dans Power BI Desktop, à partir du code du générateur, qui fait référence. **À refaire à chaque changement de version des fonctions**, puis mettre à jour le tableau du haut.

### 0. Prérequis : une adresse de référentiel qui fonctionne, le temps de la construction

`P_Chemin_Referentiel` contient une adresse d'exemple fictive : avec elle, le référentiel ne se charge pas. Pendant la construction, mettre l'adresse d'une copie qui fonctionne (votre OneDrive, ou la copie GitHub du dépôt). Pas besoin de la retirer avant l'export : un .pbit n'enregistre pas la valeur des paramètres (vérifié sur le fichier exporté, où chaque paramètre vaut `null`). Chaque utilisateur saisit la sienne à l'ouverture.

### 1. Partir d'un fichier vierge

- Power BI Desktop › Fichier › **Nouveau**. Ne jamais partir d'un ancien rapport ou d'un gabarit existant : thème, logo et requêtes cachées seraient emportés dans le modèle.
- Fichier › Options et paramètres › Options › **Fichier actif › Chargement des données** : décocher « Date/heure automatique » (sinon Power BI ajoute des tables de dates cachées).

### 2. Créer les 7 requêtes, dans l'ordre

Ouvrir `generateur_api.html`, onglet **Fonctions**. Pour chaque requête : Accueil › Transformer les données › Nouvelle source › **Requête vide** › Éditeur avancé, coller le code, OK, puis renommer exactement. Ensuite, clic droit sur la requête › **Propriétés** › **Description** : coller la description affichée dans le cadre bleu au-dessus du code (bouton « Copier la description »). Elle s'affiche en info-bulle dans le volet des requêtes, et dans la fenêtre des paramètres à l'ouverture du modèle.

| Ordre | Nom | Vérification |
|---|---|---|
| 1 | `fn_MelodiDico` | Icône de fonction (fx) ; aide de la fonction : version 1.2 |
| 2 | `fn_MelodiData` | Icône de fonction (fx) ; aide de la fonction : version 1.5 |
| 3 | `P_Millesime_RP` | Icône de paramètre, valeur `2023` |
| 4 | `P_Departements_Perimetre` | Icône de paramètre, valeur `67,68` |
| 5 | `P_Chemin_Referentiel` | Icône de paramètre. Le temps de la construction : remplacer l'adresse d'exemple par une adresse qui fonctionne (étape 0) |
| 6 | `DIM_Referentiel_Geographique` | Table de 37 304 lignes |
| 7 | `P_Communes_Perimetre` | Liste de 880 éléments (`COM-67001`…) pour 67 et 68 |

Clic droit › décocher **Activer le chargement** pour `P_Communes_Perimetre` (c'est une liste) et pour les trois paramètres `P_Millesime_RP`, `P_Departements_Perimetre`, `P_Chemin_Referentiel`. Seules `DIM_Referentiel_Geographique` et la requête d'exemple sont chargées dans le modèle.

### Descriptions à coller (Propriétés › Description)

| Requête | Description |
|---|---|
| `fn_MelodiDico` | Fonction : dictionnaire d'un jeu Melodi (Insee), codes et libellés de chaque variable, niveau géographique de chaque territoire (NIVEAU_GEO). Paramètre : nom du jeu (DS_…). |
| `fn_MelodiData` | Fonction : données d'un jeu Melodi (Insee), filtrées côté API, interrogées par paquets de 250 territoires, avec nouvel essai en cas de limite d'appels. Erreur explicite si aucune donnée. Paramètres : jeu, filtres, libellés à la place des codes. |
| `P_Millesime_RP` | Paramètre : millésime du recensement de la population utilisé par les requêtes RP (ex. 2023). |
| `P_Departements_Perimetre` | Paramètre : codes des départements du périmètre, séparés par des virgules (ex. 67,68 ; 2A,2B). Vide : France entière, déconseillé. |
| `P_Chemin_Referentiel` | Paramètre : adresse web du classeur Referentiel_Geographique_2026.xlsx (copie SharePoint ou OneDrive, lien direct vers le fichier). Essai rapide : https://raw.githubusercontent.com/lolita-debug/generateur_api/main/referentiel/Referentiel_Geographique_2026.xlsx |
| `DIM_Referentiel_Geographique` | Table : communes actuelles et historiques (Insee, COG 2026) avec leurs zonages. Clé : Code commune historique. Relier les données Melodi par CODE_GEO. |
| `P_Communes_Perimetre` | Liste : codes COM-xxxxx des communes actuelles du périmètre, envoyés à l'API comme filtre GEO. Ne pas charger dans le modèle. |
| `FAIT_RP_Population_AgeDetaille_Alsace` | Exemple : population des 18 à 25 ans et population totale des communes du périmètre (recensement, API Melodi). Peut être supprimée. |

### 3. Ajouter la requête d'exemple

Nouvelle requête vide nommée `FAIT_RP_Population_AgeDetaille_Alsace`, avec ce code (produit par le générateur, exemple « Population 18-25 ans »), et la description du tableau ci-dessus.

```
let
    #"Données du périmètre extraites" =
        fn_MelodiData(
            "DS_RP_TD_POPULATION_AGESEX_PRINC",
            [
                TIME_PERIOD = P_Millesime_RP,
                SEX         = "_T",
                AGE         = {"Y18", "Y19", "Y20", "Y21", "Y22", "Y23", "Y24", "Y25", "_T"},
                GEO         = P_Communes_Perimetre
            ]
        ),

    // Colonnes retirées : GEO (code complet, CODE_GEO sert à la jointure), NIVEAU_GEO,
    // variables filtrées sur un seul code (SEX)
    // Une variable du jeu non filtrée reste visible : la filtrer avant toute somme
    #"Colonnes inutiles retirées" =
        Table.RemoveColumns(
            #"Données du périmètre extraites",
            {"GEO", "NIVEAU_GEO", "LIB_TIME_PERIOD", "SEX", "LIB_SEX"},
            MissingField.Ignore
        )
in
    #"Colonnes inutiles retirées"
```

### 4. Charger et relier

- **Fermer et appliquer.** Identifiants : voir le tableau « Utilisation », étape 4. Les deux sources doivent avoir le même niveau de confidentialité.
- Vue Modèle : relation `FAIT_RP_Population_AgeDetaille_Alsace[CODE_GEO]` (plusieurs) → `DIM_Referentiel_Geographique[Code commune historique]` (un), filtrage dans un seul sens.
- Page du rapport : une zone de texte suffit, par exemple « Modèle API Melodi. Mode d'emploi : github.com/lolita-debug/generateur_api ». Pas de thème personnalisé.

### 5. Tester avant d'exporter

| Test | Attendu |
|---|---|
| Actualiser | Pas d'erreur. La requête d'exemple compte environ 7 900 lignes (880 communes × 9 modalités d'âge, moins les croisements absents) |
| Colonnes de l'exemple | Au moins `TIME_PERIOD`, `AGE`, `LIB_AGE`, `Valeur`, `CODE_GEO`. Une autre colonne de variable apparaît ? C'est une variable du jeu non filtrée : la noter, elle doit avoir une seule valeur |
| Relation | Dans un tableau « Libellé commune actuelle » + `Valeur`, aucune ligne « (vide) » |
| Total | Pour une commune, `AGE = _T` est supérieur à la somme de Y18 à Y25 (normal : `_T` couvre tous les âges) |
| Périmètre | Accueil › Modifier les paramètres › `P_Departements_Perimetre` = `2A,2B` › Appliquer : seules les communes corses restent |
| Erreur explicite | `P_Departements_Perimetre` = `99` (aucun département) › Appliquer : l'exemple affiche l'erreur « API Melodi : aucune donnée », et non une table vide |

Remettre ensuite `P_Departements_Perimetre` à `67,68`.

### 6. Exporter

- Vérifier avant d'exporter : vue Modèle, seules `DIM_Referentiel_Geographique` et `FAIT_RP_Population_AgeDetaille_Alsace` apparaissent, reliées ; Options › Fichier actif › Chargement des données : « Date/heure automatique » décoché.
- Fichier › **Exporter › Modèle Power BI**.
- Description : « Requêtes API Melodi (Insee) prêtes à l'emploi : fonctions v1.2 et v1.5, paramètres et référentiel des communes. À l'ouverture, renseigner l'adresse de votre copie du référentiel. Mode d'emploi : github.com/lolita-debug/generateur_api »
- Enregistrer sous `Modele_API_Melodi.pbit`.
- Rouvrir le .pbit obtenu : les trois paramètres sont demandés ; avec une adresse de référentiel valide, le chargement aboutit.
