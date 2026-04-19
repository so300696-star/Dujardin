# AGENTS.md - Générateur de factures Dujardin (refonte vente + préparation TypeScript)

## 0) Contexte

Ce document décrit les modifications à appliquer au générateur de factures HTML existant utilisé par le garage Dujardin Automobile.

Objectifs :

* Simplifier le formulaire
* L’adapter à la vente de véhicules d’occasion
* Ajouter la gestion du véhicule repris
* Préparer une future migration vers TypeScript
* Conserver une utilisation rapide et simple

---

## 1) Structure du formulaire (nouvelle organisation)

Le formulaire doit être organisé dans cet ordre :

1. Informations facture
2. Client
3. Véhicule vendu
4. Véhicule repris
5. Lignes de facturation
6. Paiement
7. Observations (optionnel)

---

## 2) Modifications par section

---

### 2.1 Informations facture

#### Champs à conserver

* `f_num`
* `f_date`
* `f_cmd`

#### Champ à modifier

* `f_pay` :

  * uniquement :

    * Chèque bancaire
    * Virement bancaire

#### Champ à ajouter

* `f_livraison`

---

### 2.2 Client

#### À supprimer

* `c_name`

#### À ajouter

* `c_nom`
* `c_prenom`

#### À conserver

* `c_addr`
* `c_city`
* `c_tel`

---

### 2.3 Véhicule vendu

#### Champs à conserver

* `v_marque`
* `v_modele`
* `v_annee`
* `v_mec`
* `v_immat`
* `v_vin`
* `v_km`
* `v_energie`
* `v_boite`

#### Champs à supprimer

* `v_puiss`
* `v_couleur`
* `v_type`

---

### 2.4 Véhicule repris

#### Champ principal

* `r_has` → Oui / Non

#### Champs conditionnels

* `r_marque`
* `r_modele`
* `r_immat`
* `r_km`
* `r_montant`

#### Comportement

* Masquer si Non
* Afficher si Oui

---

### 2.5 Lignes de facturation

#### Champs à conserver

* `.l-des`
* `.l-qty`
* `.l-price`

#### Modification

* "Prix unit. TTC" → "Prix unitaire"

#### Règle

* Total calculé automatiquement

---

### 2.6 Paiement

#### Renommer

* "Coûts inclus & Totaux" → "Paiement"

#### À supprimer

* `t_cg`

#### À conserver

* `t_remise`
* `t_paye`

---

### 2.7 Observations

#### Renommer

* `t_inclus` → `t_obs`

---

## 3) Mentions fixes (PDF)

Ajouter automatiquement :

* Véhicule vendu avec garantie 6 mois
* Véhicule révisé avant la vente

---

## 4) Éléments à supprimer

* Mode de financement
* Crédit
* Email client
* Société client
* SIRET client
* Kilométrage garanti
* Champs véhicule inutiles

---

## 5) Impact JavaScript

### À supprimer

* variables inutiles

### À ajouter

* `f_livraison`
* `r_*`

### À modifier

* Nom client → Nom + Prénom
* Gestion affichage reprise

---

## 6) Impact PDF

### À ajouter

* Date de livraison
* Bloc reprise

### À modifier

* Affichage client

### À ajouter

* Mentions fixes

---

# 🧠 7) Préparation migration TypeScript (IMPORTANT)

Le code doit être modifié pour faciliter une future migration vers TypeScript.

---

## 7.1 Séparer logique et DOM

### ❌ À éviter

* Mélanger calcul + manipulation HTML

### ✅ À faire

Créer des fonctions séparées :

* récupération des données formulaire
* calculs
* rendu PDF

---

## 7.2 Créer une structure objet claire

Les données doivent être regroupées dans des objets.

### Exemple logique attendu

Facture :

* numero
* date
* dateLivraison
* modePaiement
* lignes
* remise
* montantPaye

Client :

* nom
* prenom
* adresse
* ville
* telephone

Vehicule :

* marque
* modele
* annee
* miseEnCirculation
* immatriculation
* vin
* kilometrage
* energie
* boite

VehiculeRepris :

* present (boolean)
* marque
* modele
* immatriculation
* kilometrage
* montant

---

## 7.3 Préparer le typage

Le code doit être écrit de manière compatible avec TypeScript :

### Bonnes pratiques

* variables clairement nommées
* éviter les valeurs implicites
* convertir les nombres explicitement
* éviter les valeurs undefined non contrôlées

---

## 7.4 Centraliser les calculs

Créer une logique unique pour :

* total lignes
* remise
* montant payé
* reste dû

---

## 7.5 Préparer modularisation future

Le code doit pouvoir être séparé en modules :

* gestion formulaire
* calculs
* rendu facture
* PDF

---

## 7.6 Objectif TypeScript futur

Pouvoir transformer facilement le projet en :

* fichiers `.ts`
* types stricts
* architecture modulaire
* application Electron

---

# 8) Contraintes

* garder design actuel
* garder PDF A4
* ne pas complexifier
* priorité rapidité

---

# 9) Objectif final

Obtenir un outil :

* simple
* robuste
* évolutif
* prêt pour TypeScript
* prêt pour version application (.exe)
