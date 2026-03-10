# Is Economic Uncertainty Priced in the Cross-Section of Stock Returns?

> **Mémoire de M1 – Économétrie et Ingénierie Financière (EIF)**  
> Reproduction et extension empirique de l'article de référence en finance d'actifs

---

## 📄 Article de référence

**Bali, T. G., Brownlees, C., Engle, R. F., & Gokcan, S. (2017)**  
*"Is economic uncertainty priced in the cross-section of stock returns?"*  
**Journal of Financial Economics**  

L'article examine si l'incertitude économique constitue un facteur de risque systématique qui est **tarifié dans la coupe transversale des rendements boursiers américains**. La question centrale : les investisseurs exigent-ils une prime pour détenir des actions fortement exposées à l'incertitude macro-économique ?

---

## 🧠 Problématique

> **L'incertitude économique est-elle une source de risque systématique qui explique les différences de rendements entre les actions ?**

L'hypothèse principale testée est qu'un coefficient β_UNC négatif et significatif dans les régressions Fama–MacBeth traduirait une **prime refuge** : les investisseurs acceptent des rendements plus faibles sur les actifs qui protègent contre l'incertitude macroéconomique.

---

## 📁 Structure du dépôt

```
Economic-Uncertainty-Priced-in-Stock-Returns/
│
├── Code/
│   └── Mémoire_M1_EIF-2.ipynb          # Notebook principal – reproduction empirique complète
│
├── Data/                                # Données utilisées (JLN, CRSP, Fama-French)
│
├── Soutenance/
│   └── Méthodo Mémoire.ipynb           # Fiche méthodologique détaillée + glossaire
│
├── Is economic uncertainty priced in the cross-section of stock returns_.pdf
│                                        # Article original
└── README.md
```

---

## ⚙️ Méthodologie

La reproduction suit scrupuleusement la méthodologie de l'article original, en 8 étapes clés :

### 1. 📊 Construction de la mesure d'incertitude (indice JLN)

- **Indicateur** : Indice d'incertitude macroéconomique de **Jurado, Ludvigson et Ng (JLN)**
- Agrège l'imprévisibilité (volatilité inattendue) de nombreuses séries macroéconomiques US : production, emploi, inflation, etc.
- Calcul mensuel de la part d'incertitude **non expliquée** par des modèles autorégressifs
- Horizons disponibles : 1, 3 et 12 mois — horizon **1 mois** principalement utilisé

### 2. 📐 Estimation du bêta d'incertitude (β_UNC) pour chaque action

- **Fenêtre glissante de 60 mois** : pour chaque action *i* et chaque mois *t*, estimation sur les 60 mois précédents
- **Régression de référence (OLS)** :

$$
R_{i,\tau} - R_{f,\tau} = \alpha_{i,t} + \beta_{UNC,i,t} \cdot UNC_\tau + \beta_{MKT,i,t} \cdot MKT_\tau + \varepsilon_{i,\tau}
$$

- Résultat : un β_UNC dynamique actualisé chaque mois pour chaque action

### 3. 📂 Formation des portefeuilles

- Tri mensuel des actions selon leur β_UNC estimé
- Constitution de **10 déciles** (du plus faible au plus fort β_UNC)
- Deux types de pondération : **égal-pondéré** et **valeur-pondéré** (par capitalisation boursière)
- Portefeuille long-short : **décile 10 − décile 1**

### 4. 📈 Calcul des rendements et alphas des portefeuilles

- Rendements excédentaires moyens pour chaque décile
- **Alphas factoriels** estimés par rapport à :
  - CAPM (1 facteur)
  - Fama-French 5 facteurs (marché, taille, valeur, momentum, liquidité)
  - Fama-French-Carhart 7 facteurs (+investissement et rentabilité)
- Correction des erreurs standard : **Newey–West** (robustesse à l'autocorrélation et à l'hétéroscédasticité)

### 5. 🔀 Tris bivariés conditionnels

- **Tri primaire** : découpage selon une caractéristique (liquidité ILLIQ, taille, valeur, etc.)
- **Tri secondaire** : dans chaque groupe, nouveau tri en déciles selon β_UNC
- Objectif : vérifier si l'effet β_UNC subsiste après contrôle d'autres caractéristiques

### 6. 📉 Régressions Fama–MacBeth

Pour chaque mois *t*, régression cross-sectionnelle :

$$
 y_{i,t+1} = \gamma_{0,t} + \gamma_{1,t} \cdot \beta_{UNC,i,t} + \sum_k \gamma_{k+1,t} X_{k,i,t} + \varepsilon_{i,t+1}
$$

- Variables de contrôle firm-level : taille, B/M, momentum, illiquidité, volatilité idiosyncratique
- Contrôle des effets sectoriels (variables muettes d'industrie)
- Significativité testée avec t-statistiques corrigées **Newey-West**
- Un coefficient γ₁ **négatif et significatif** confirme la prime refuge

### 7. 🧪 Tests de robustesse

- Différents horizons de prévision : 1, 3, 6, 12 mois
- Spécifications alternatives du modèle
- Analyses par **sous-périodes** (avant/après crise 2008, Covid-19)
- Gestion rigoureuse des valeurs manquantes

---

## 📦 Données utilisées

| Source | Description |
|--------|-------------|
| **JLN** | Indice d'incertitude macroéconomique (Jurado, Ludvigson & Ng) |
| **CRSP** | Rendements mensuels des actions US, capitalisation boursière |
| **Kenneth French Data Library** | Facteurs Fama-French (MKT, SMB, HML, MOM, RMW, CMA) |
| **FRED / autres** | Taux sans risque mensuel |

---

## 🛠️ Stack technique

| Outil | Usage |
|-------|-------|
| **Python 3.12** | Langage principal |
| **Jupyter Notebook** | Environnement d'analyse interactive |
| **pandas** | Manipulation des données en panel |
| **numpy** | Calculs matriciels et statistiques |
| **statsmodels** | Régressions OLS, Fama-MacBeth, Newey-West |
| **matplotlib / seaborn** | Visualisation des résultats |

---

## 🚀 Lancement

### Prérequis

```bash
pip install pandas numpy statsmodels matplotlib seaborn jupyter
```

### Exécution

```bash
# Cloner le dépôt
 git clone https://github.com/Mariuscalaque/Economic-Uncertainty-Priced-in-Stock-Returns.git
 cd Economic-Uncertainty-Priced-in-Stock-Returns

# Lancer Jupyter
 jupyter notebook Code/Mémoire_M1_EIF-2.ipynb
```

> ⚠️ Les données brutes (CRSP, Compustat) nécessitent un accès institutionnel (WRDS). Les données JLN sont disponibles publiquement sur le site de Sydney Ludvigson.

---

## 📊 Résultats attendus

Conformément à l'article original :

- Les actions avec un **β_UNC élevé** (forte sensibilité à l'incertitude) tendent à afficher des **rendements futurs plus faibles**
- L'effet est **robuste** aux contrôles habituels (taille, valeur, momentum, liquidité)
- Les régressions Fama–MacBeth confirment une **prime de risque d'incertitude négative** et statistiquement significative
- L'interprétation économique : ces actions jouent le rôle de **couverture contre l'incertitude macro**, ce qui justifie une prime refuge

---

## 📚 Références

- Bali, T. G., Brownlees, C., Engle, R. F., & Gokcan, S. (2017). *Is economic uncertainty priced in the cross-section of stock returns?* Journal of Financial Economics.
- Jurado, K., Ludvigson, S. C., & Ng, S. (2015). *Measuring Uncertainty.* American Economic Review, 105(3), 1177–1216.
- Fama, E. F., & MacBeth, J. D. (1973). *Risk, return, and equilibrium: Empirical tests.* Journal of Political Economy, 81(3), 607–636.
- Fama, E. F., & French, K. R. (1993, 2015). *Common risk factors in the returns on stocks and bonds / A five-factor asset pricing model.*

---

## 👤 Auteur

**Marius Calaque**  
M1 Économétrie et Ingénierie Financière  
*Mémoire de recherche – Reproduction d'article académique*
