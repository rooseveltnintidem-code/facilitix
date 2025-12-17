# FACILITIX - Calculateur de Devis Export

## 1. Présentation

> **FACILITIX** est un outil web destiné à simplifier le calcul et la génération instantanée de devis d’export logistique pour les PME camerounaises. Grâce à son interface intuitive, l'utilisateur renseigne quelques informations clés (marchandises, mode de transport, port, volumes...) et obtient un devis clair, détaillé, incluant tous les frais (logistiques, documentaires, spécifiques produits ou ports).

---

## 2. Logique Métier - Calcul des Coûts

### 2.1. Frais inclus systématiquement

- **Frais Portuaires** (ex: acconage, mise sur camion, redevances…)
- **Frais de Manutention** (ex: positionnement, double relevage, transfert, pesée…)
- **Frais de Transport** (charges selon mode & destination)
- **Frais Documentaires** (Bordereau, Timbre BL, TEL, Vacations douane/BAE)
- **Frais Spécifiques Produit** (filières cacao/café, bois, etc.)
- **TVA** : 19,25% du total hors taxe

Les barèmes détaillés (2025) dépendent :
- Du **port d’embarquement** (Douala, Kribi)
- Du **mode de transport** (Maritime FCL - conteneur complet, Maritime LCL - groupage, Aérien)
- Du **type de produit** (général, cacao/café, bois)
- Des **quantités** (nombre de conteneurs, poids, etc.)

---

### 2.2. Algorithme simplifié

1. **Sélection du port** ⇒ récupération du barème correspondant
2. **Choix du mode de transport** ⇒ logique de calcul spécifique (voir ci-après)
3. **Saisie marchandises** : poids, nombre/type de conteneur(s), produit
4. **Addition de tous les coûts fixes + variables**
5. **Application de tous les frais documentaires** communs
6. **Calcul des frais spécifiques liés au produit, si applicable**
7. **Calcul des sous-totaux** par catégorie (port, manutention, documentation, spécifiques, transport)
8. **Calcul du total HT**
9. **Application de la TVA calculée**
10. **Affichage du total TTC + détail par poste**

---

### 2.3. Barèmes & Formules par Mode de Transport

#### A. **Maritime FCL (Conteneur complet)**
- Coûts **portuaires/manutention** : par port & type de conteneur (20"/40") × nb conteneurs
- Frais documentaires : communs à tous les cas
- Transport interne : selon tarif local
- Spécifiques produits (ex. cacao, bois) : frais additionnels par conteneur

#### B. **Maritime LCL (Groupage)**
- Coût manutention : tarif à la tonne, min. forfait
- Frais portuaires/documentaires adaptés à la LCL

#### C. **Aérien**
- Coût : tarif/kg, min. de facturation
- Frais de manutention/documentation adaptés à l’aérien

---

## 3. Exemples de Calculs Concrets

### **Exemple 1 - Maritime FCL (20 pieds) - Douala - Produits Généraux**

- **Port** : Douala
- **Mode** : Maritime FCL
- **Type de conteneur** : 20 pieds
- **Nb containers** : 2
- **Poids total** : 34 000 kg
- **Produit** : Générale

#### Calcul :

| Poste                          | Montant (FCFA)          |
|---------------------------------|-------------------------|
| Acconage (2x101630)             | 203 260                |
| Mise sur camion (2x39600)       | 79 200                 |
| Redevance sécurité (2x2000)     | 4 000                  |
| Positionnement (2x60 000)       | 120 000                |
| Double relevage (2x40000)       | 80 000                 |
| Transfert RTC (2x75 000)        | 150 000                |
| Ticket pesée (2x10000)          | 20 000                 |
| Empotage forfait (2x50 000)     | 100 000                |
| Revetement TC (2x25000)         | 50 000                 |
| **Total Frais portuaires+manut**| **806 460**            |
| **Documentation**               | 40 000+25 000+40 000+1 500+5 000+5 000 + 2x50 000 + 2x10 000 = **236 500** |
| **Spécifiques produits**        | 0                      |
| **Sous-total HT**               | **1 042 960**          |
| **TVA (19,25%)**                | 1 042 960 × 0,1925 = **200 760**  |
| **Total TTC**                   | **1 243 720**          |

---

### **Exemple 2 - Maritime LCL (Groupage) - Douala - Cacao**

- **Port** : Douala  
- **Mode** : Maritime LCL  
- **Poids total** : 2500 kg  
- **Produit** : Cacao  

#### Calcul :

| Poste                          | Montant (FCFA)          |
|--------------------------------|-------------------------|
| Manutention (2,5T x 6 331)     | 15 827,5 → min. 6 331 x 1 = **6 331** (si min forfait, sinon 15 827) |
| Dépotage chariot               | 90 000                  |
| Documentation                  | 40 000 + 25 000 = **65 000**  |
| Spécifiques Cacao              | 100 000 (TEL) + 150 000 (interventions) + 11 500 = **261 500** |
| **Sous-total HT**              | 6 331 + 90 000 + 65 000 + 261 500 = **422 831** |
| **TVA (19,25%)**               | 81 387                  |
| **Total TTC**                  | **504 218**             |

---

### **Exemple 3 - Aérien - Douala - Bois**

- **Port** : Douala  
- **Mode** : Aérien  
- **Poids total** : 2 400 kg  
- **Produit** : Bois et dérivés  

#### Calcul :

| Poste                                   | Montant (FCFA)      |
|------------------------------------------|---------------------|
| Transport (max(2400x150, 50000))         | 360 000             |
| Manutention                              | 25 000              |
| Documentation                            | 25 000              |
| Spécifiques Bois                         | 60 000 (enreg.) + 30 000 (deleg) + 45 000 (sign. difor) + 20 000 (ecor difor) + 11 500 = **166 500** |
| **Sous-total HT**                        | 360,000 + 25,000 + 25,000 + 166,500 = **576,500** |
| **TVA (19,25%)**                         | 110,885             |
| **Total TTC**                            | **687,385**         |

---

## 4. FAQ & Cas particuliers

- **Cumul des catégories** : Tous les postes sont additionnés dans le tableau final du devis.
- **Valeurs à modifier/flexibiliser** : Au besoin, les barèmes peuvent être ajustés dans le fichier de paramétrage métier (`logistics_cost_matrix.txt`).
- **Export CSV** : Chaque devis généré est exportable au format CSV pour archivage ou facturation.
- **Mobile-first** : L’interface est pensée responsive et accessible sur mobile.

---

## 5. Hébergement Suggéré

- **Rapide & scalable (préconisé)** : [Vercel](https://vercel.com/) ou [Netlify](https://netlify.com/) (déploiement ultra-simple, CDN mondial, version gratuite adaptée à une PME)
- **Infrastructure locale** : OVH Web Cloud, AlwaysData, ou Azure France pour hébergement sur serveurs privés/réglementés

---

## 6. Fonctionnalités à intégrer sur le frontend

- **Formulaire étape unique** : port, mode, type de produit, quantités/poids
- **Affichage dynamique du calcul détaillé** (tableau par poste)
- **Export CSV du résultat**
- **Responsive design** : conforme mobile & desktop
- **Aide contextuelle (“tooltips”)** sur chaque champ clé

---

## 7. Informations complémentaires à fournir au développeur

- **Lien du dépôt Github ou accès à la plateforme d’intégration**
- **Barèmes tarifaires à jour** (modifiables par le métier si besoin)
- **Textes légaux, CGU, mentions obligatoires, si requis**

---

**Contact Product Owner** :  
rooseveltnintidem-code (github)  
[Ajoutez votre email ici si besoin]

---

**Merci de transmettre ce document à tout développeur ou designer intervenant sur le projet !** Vous pouvez enrichir par des schémas d’UI selon les besoins métier.