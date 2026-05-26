# Dataset — NeoBank CLV Analytics

Les données utilisées dans ce projet sont **entièrement synthétiques**.
Elles simulent un environnement bancaire américain fictif sur la période 2020–2025.

Aucune donnée personnelle réelle n'a été utilisée.

---

## Volume & Couverture

| Indicateur | Valeur |
|------------|--------|
| Transactions | **10 000** |
| Clients | **100** |
| Comptes | **193** |
| Produits | **27** |
| Période | **Janvier 2020 → Septembre 2025** |
| Régions | **8 États américains** |

---

## Tables

### `FactTransaction` — 10 000 lignes
Table centrale du modèle en étoile.

| Colonne | Description |
|---------|-------------|
| `TransactionID` | Identifiant unique de la transaction |
| `AccountID` | Lien vers DimAccount |
| `TransactionDate` | Date de la transaction (2020–2025) |
| `TransactionAmount` | Montant (peut être négatif = débit) |
| `TransactionType` | Credit / Debit |
| `TransactionChannel` | Web · ATM · Mobile |
| `ProductID` | Lien vers DimProduit |
| `Status` | Success / Failed |

---

### `DimCustomer` — 100 clients
| Colonne | Description |
|---------|-------------|
| `CustomerID` | Identifiant unique |
| `FullName` | Nom fictif |
| `DOB` | Date de naissance |
| `Gender` | Male (53%) · Female (47%) |
| `Region` | 8 États : Florida, California, Alaska, Colorado, Kansas, New Mexico, Massachusetts, Utah |
| `Status` | Active · Inactive · Suspended |
| `JoinDate` | Date d'adhésion |

---

### `DimAccount` — 193 comptes
*(Certains clients ont plusieurs comptes)*

| Colonne | Description |
|---------|-------------|
| `AccountID` | Identifiant unique |
| `CustomerID` | Lien vers DimCustomer |
| `AccountType` | Credit · Checking · Savings |
| `OpenDate` / `ClosedDate` | Dates d'ouverture et fermeture |
| `Status` | Open (86) · Closed (107) |
| `Balance` | Solde (-9 871 → +48 604) |

---

### `DimProduit` — 27 produits
Hiérarchie : **3 catégories → 9 sous-catégories → 27 produits**

| Catégorie | Sous-catégories | Produits |
|-----------|----------------|---------|
| A | AA, AB, AC | AAA, AAB, AAC, ABA... |
| B | BA, BB, BC | BAA, BAB, BAC, BBA... |
| C | CA, CB, CC | CAA, CAB, CAC, CBA... |

---

### `Table_RFM` — Calculée en DAX
Générée à partir de FactTransaction. Contient les scores RFM et la CLV par client.

| Colonne | Description |
|---------|-------------|
| `R_Score` | Score Récence (1–5) |
| `F_Score` | Score Fréquence (1–5) |
| `M_Score` | Score Monétaire (1–5) |
| `Segment_Client` | Champions · Clients Standard · À Risque · Nouveaux Clients · Perdus |
| `CLV_Moyenne` | Valeur vie client calculée |

---

### `Produits_Comparaison` — Calculée en DAX
Matrice de co-achat pour le basket analysis (27 × 27 produits).