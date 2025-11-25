# **Rapport de Projet - PoketraFinday**

---

## **Examen Final Machine Learning & Data Science**

Réalisé au sein de ISPM - Madagascar (www.ispm-edu.com)

### **1. Informations sur le Groupe**

Merci de lister tous les membres de l'équipe ayant participé au Hackathon.

#### Membre 1 :
* **nom** : RASOLOFOARIJAONA
* **prénom(s)** : Eric
* **classe** : IMTICIA 5A
* **numéro** : 02
* **rôle** : développeur

#### Membre 2 :
* **nom** : ANDRIANANTENAINA
* **prénom(s)** : Mialy Riantsoa
* **classe** : IMTICIA 5A
* **numéro** : 12
* **rôle** : analyste

#### Membre 3 :
* **nom** : RAKOTONANDRASANA
* **prénom(s)** : Laethicia Prisca
* **classe** : IMTICIA 5A
* **numéro** : 04
* **rôle** : développeur

#### Membre 4 :
* **nom** : VALISOA
* **prénom(s)** : Mampionona
* **classe** : ESIIA 5A
* **numéro** : 03
* **rôle** : analyste

#### Membre 5 :
* **nom** : RAKOTOSON
* **prénom(s)** : Finiavana lucas
* **classe** : ESIIA 5A
* **numéro** : 26
* **rôle** : analyste

---

### **2. Résumé du Travail**

## Problématique :

* **PoketraFinday** fait face à une augmentation des transactions potentiellement frauduleuses, mettant en risque la sécurité des utilisateurs et la fiabilité du service financier. L’absence de mécanismes avancés de détection rend la plateforme vulnérable aux comportements anormaux et au détournement de micro-crédits. Résoudre ce problème est critique pour protéger les clients, limiter les pertes financières et renforcer la confiance dans l’écosystème numérique de la fintech.

## Méthodologie Adoptée :

* Nous avons d’abord réalisé une **analyse exploratoire (EDA)** pour identifier les tendances, outliers et patterns de fraude dans les données. Un **pré-traitement** spécifique a été appliqué : gestion du déséquilibre, encodage des variables catégorielles, suppression des champs non pertinents et normalisation lorsque nécessaire. Plusieurs modèles ont été testés (Logistic Regression, RandomForest, **XGBoost**) avec une **validation croisée (Stratified K-Fold)** pour garantir une évaluation robuste. L’optimisation des hyper-paramètres a permis d'améliorer la détection tout en limitant les faux positifs.

## Résultats Obtenus :

* Le meilleur modèle (**XGBoost**) a obtenu un **F1-Score de 0.8171** sur le jeu de validation, démontrant une capacité équilibrée à identifier les transactions frauduleuses. Le modèle a également atteint une **Précision de 0.8228**, un **Rappel de 0.8115** et un **AUC-ROC de 0.9012**. Une découverte clé : certaines variables, notamment celles liées au risque (comme `merchant_risk_score`), à la localisation (`transaction_country`) et les patterns temporels, se sont révélées particulièrement discriminantes pour détecter la fraude.

## Mots-clés :

* Fraude, Machine Learning, Imbalanced Data, XGBoost, Financial Security

---

### **3. Contenu du Repository**

Voici la liste des fichiers et liens importants pour évaluer notre travail :

* **`notebook.ipynb`** : Le code complet (EDA, Preprocessing, Modélisation) avec commentaires.
* **`submission.csv`** : Nos prédictions sur le fichier `test.csv`.
* **`readme.md`** : Ce présent rapport.
* **`final_model.pkl`**
* **`scaler.pkl`**
* **🔗 Liens Utiles :**
  https://drive.google.com/file/d/1Ce9ffIovBVVnoIp9s8hRtLNjdMydQgVk/view?usp=sharing

---

### **4. Réponses aux Questions d'Analyse**

**Q1. Pourquoi on utilise F1-Score au lieu de accuracy ?**

**1. Le Problème du Déséquilibre de Classe**
Dans la détection de fraude, le jeu de données est très **déséquilibré** :
   * *Transactions Légitimes (Classe Négative)* : Plus de **99 %** des cas.
   * *Transactions Frauduleuses (Classe Positive)* : Moins de **1 %** des cas.

Si vous utilisiez l'**Accuracy**, un modèle pourrait obtenir un score très élevé (par exemple, 99 %) en prédisant simplement que toutes les transactions sont légitimes. Ce modèle serait inutile car il ne détecterait aucune fraude, mais son Accuracy serait trompeuse.

**L'Accuracy se calcule comme suit :**
$$\text{Accuracy} = \frac{\text{Vrais Positifs} + \text{Vrais Négatifs}}{\text{Total des Observations}}$$

**2. L'Importance de la Précision et du Rappel**

Le **F1-Score** contourne ce problème en combinant deux métriques essentielles qui se concentrent sur la performance de la classe minoritaire (la fraude) : la **Précision** et le **Rappel**.

**A. Le Rappel (Recall/Sensibilité)**

Le Rappel mesure la capacité du modèle à détecter **toutes les fraudes réelles**.
$$\text{Rappel} = \frac{\text{Vrais Positifs}}{\text{Vrais Positifs} + \text{Faux Négatifs}}$$

    > **Coût d'un Faux Négatif (FN)**: C'est une vraie fraude manquée par le modèle. Le coût est **direct (perte financière) et élevé**.
    > **Objectif**: Maximiser le Rappel pour minimiser les pertes financières dues aux fraudes non détectées.

**B. La Précision (Precision)**

La Précision mesure la **fiabilité des prédictions de fraude** : parmi toutes les transactions que le modèle a signalées comme fraudes, combien étaient réellement frauduleuses.
$$\text{Précision} = \frac{\text{Vrais Positifs}}{\text{Vrais Positifs} + \text{Faux Positifs}}$$

    > **Coût d'un Faux Positif (FP)**: C'est une transaction légitime signalée à tort comme fraude (fausse alarme). Le coût n'est pas financier, mais **opérationnel** (inspection manuelle, blocage du client, impact sur l'expérience utilisateur) et peut être coûteux en temps et en ressources.
    > **Objectif**: Maximiser la Précision pour minimiser les fausses alertes et le travail d'investigation inutile.

**3. Le Rôle du F1-Score (Moyenne Harmonique)**

Le F1-Score est la **moyenne harmonique** de la Précision et du Rappel. Il est calculé comme suit :
$$\text{F1-Score} = 2 \times \frac{\text{Précision} \times \text{Rappel}}{\text{Précision} + \text{Rappel}}$$

Le F1-Score est le meilleur indicateur car il cherche un **équilibre** :

* Un modèle avec un Rappel parfait (100 %) mais une Précision très faible (beaucoup de fausses alertes) aura un F1-Score médiocre.
* Un modèle avec une Précision parfaite (100 %) mais qui manque la moitié des fraudes (Rappel faible) aura aussi un F1-Score médiocre.

En optimisant le F1-Score, on s'assure que le modèle est non seulement capable de détecter un maximum de fraudes (**bon Rappel**), mais que ses alertes sont également fiables (**bonne Précision**).

---

**Q2. Qu'est ce qui est plus grave ici, les Faux Positifs ou les Faux Négatifs ?**

Dans le contexte de PoketraFinday (une plateforme de services financiers où le risque est la perte d'argent), les **Faux Négatifs** sont généralement considérés comme **plus graves** que les Faux Positifs.

* **Faux Négatif (FN)**: Une **vraie fraude** est classée comme légitime. **Conséquence** : **Perte financière directe** pour l'entreprise ou le client.
* **Faux Positif (FP)**: Une **vraie transaction légitime** est classée comme fraude. **Conséquence** : Inconfort pour le client (transaction bloquée) et coût opérationnel d'une vérification manuelle inutile.

Le coût direct de la perte d'argent due à une fraude non détectée (FN) surpasse le coût indirect d'une fausse alerte (FP). Par conséquent, on cherchera à **minimiser les Faux Négatifs** (c'est-à-dire **maximiser le Rappel**), même si cela entraîne une légère augmentation des Faux Positifs.

---

**Q3. Stratégie de Modélisation : Quelles nouvelles variables (Feature Engineering) ont le plus amélioré votre modèle par rapport à la Baseline ?**

La stratégie de modélisation a été considérablement améliorée grâce à l'ajout de variables dérivées du **comportement historique des clients**. Ces nouvelles variables (**Feature Engineering**) ont permis au modèle final (XGBoost) de surpasser la baseline.

Les Variables Clés du Feature Engineering

Les variables les plus puissantes sont celles qui capturent une **anomalie par rapport à l'historique de chaque client** (`customer_id`).

**1. Variables de Ratio d'Anomalie (Top Impact)**

Ces variables mesurent si la transaction actuelle est inhabituelle pour le client donné. Elles sont de loin les plus importantes après le type de transaction (`type`).

* `amount_vs_customer_max` (Importance: 0.2031):
    * Ce ratio compare le montant de la transaction en cours au montant maximum qu'un client a jamais transigé.
    * **Intérêt** : Un ratio proche de 1 (ou supérieur à 1) signale une transaction inhabituellement élevée, ce qui est un indicateur de fraude par prise de contrôle de compte.

* `amount_vs_customer_mean` (Importance: 0.0507):
    * Ce ratio compare le montant actuel à la moyenne des transactions du client.
    * **Intérêt** : Un montant largement supérieur à la moyenne habituelle du client est un signal d'alerte fort.

**2. Variables d'Agrégation sur le Client (Très Important)**

Ces variables caractérisent le comportement financier général du client et sont directement utilisées pour calculer les ratios ci-dessus.

* `customer_mean_amount` (Importance: 0.1733):
    * La valeur moyenne des transactions effectuées par ce client.
* `customer_total_amount` (Importance: 0.0389):
    * Le montant total cumulé transigé par ce client.
* `customer_min_amount` (Importance: 0.0326):
    * La valeur minimale des transactions effectuées par ce client.

**3. Variables Temporelles et Catégorielles**

Les caractéristiques liées au moment de la transaction ont également contribué à l'amélioration du modèle:

* `day_of_week` : Le jour de la semaine.
* **Indicateurs d'heure** (`is_morning`, `is_afternoon`, `is_evening`, `is_night`) : Des indicateurs booléens (0/1) basés sur l'heure de la transaction (`step`), qui permettent de détecter les fraudes ayant lieu en dehors des heures normales d'activité.

---

**Q4. Enoncez tous les types de fraudes que vous avez décelé lors de votre analyse**

L'analyse exploratoire des données (EDA) a permis d'identifier que la fraude sur la plateforme PoketraFinday est concentrée sur **deux types de transactions seulement**.
Les types de fraudes décelés sont donc :

**1. TRANSFERT (TRANSFER)**

* **Description du risque** : Fraude par virement de fonds d'un compte client vers un autre compte.
* **Gravité** : Ce type de transaction présente le taux de fraude le plus élevé (**7,29%**). Il est souvent le prélude à une extraction rapide des fonds.

**2. RETRAIT D'ARGENT (CASH_OUT)**

* **Description du risque** : Fraude par extraction physique ou par conversion de fonds vers l'extérieur du système.
* **Gravité** : Bien qu'il ait un taux de fraude plus faible (**3,17%**), c'est le type qui représente le **plus grand nombre absolu** de transactions frauduleuses dans le jeu de données analysé (**309 transactions**).

Les autres types de transactions (**PAYMENT, CASH\_IN et DEBIT**) n'ont présenté **aucune transaction frauduleuse** (0,00% de taux de fraude).

---

**Q5. Selon vous, quelle décision prendre si une transaction *en cours* est détectée comme *fraude* par le modèle ?**

**1. Bloquer la transaction immédiatement.**
* Prévenir la perte financière immédiate. C'est l'action la plus agressive et nécessaire pour les cas hautement probables de fraude (Vrai Positif potentiel).
**2. Alerter le client par canal sécurisé (SMS ou notification).**
* Informer le client que sa transaction a été refusée pour sa sécurité et lui donner une option de confirmation (par exemple, "Répondez OUI si cette transaction est légitime").
**3. Engager l'équipe Anti-Fraude.**
* Si le montant est élevé, une vérification manuelle immédiate est déclenchée pour valider le Vrai Positif et lancer une enquête si nécessaire.

---

### **5. Bibliographie**

Recherche Google et chatgpt
