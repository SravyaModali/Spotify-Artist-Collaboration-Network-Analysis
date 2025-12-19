# Spotify Artist Collaboration Network Analysis

## Overview
This project analyzes the Spotify artist collaboration network to understand how an artist’s **position in the collaboration graph** relates to their **popularity**. Instead of relying only on follower count, the analysis explores whether network influence and connectivity provide better insight into artist success.

Each artist is modeled as a node, and each collaboration between artists forms an edge, resulting in a large-scale social network. The project combines **network analysis** and **machine learning** to uncover influence patterns within the Spotify ecosystem.

---

## Motivation
This project was completed as part of my coursework and was inspired by research concepts such as **TwitterRank** and the *“million-follower fallacy,”* which show that popularity alone does not always reflect true influence. These ideas motivated me to explore whether similar patterns exist among Spotify artists and whether collaboration structure plays a meaningful role in popularity.

---

## Dataset
- **Artists:** 153,327 nodes  
- **Collaborations:** 300,386 edges  

### Node Features
- `spotify_id`
- `name`
- `followers`
- `popularity`
- `genres`
- `chart_hits`

### Edge Data
- `id_0`, `id_1` representing artist collaborations

Basic data cleaning was performed, including removing a small number of records with missing follower values.

---

## Data Preprocessing
- Loaded and processed data using **PySpark**
- Constructed the full collaboration graph using **NetworkX**
- Since the full graph was too large for efficient analysis, a **20,000-node BFS (Breadth-First Search) sample** was selected
- BFS sampling was chosen because it preserves real collaboration paths and network structure better than random sampling
- Verified representativeness by comparing degree distributions between the full graph and the BFS sample

---

## Network Analysis
The following centrality measures were computed:

- **Degree Centrality** – number of direct collaborations
- **Betweenness Centrality** – artists acting as bridges between groups
- **Closeness Centrality** – how quickly an artist can reach others
- **Eigenvector Centrality** – influence based on connections to influential artists
- **PageRank** – influence through strong or repeated collaborations

### Key Observations
- Artists such as **MC Gw**, **MC Lan**, **Gucci Mane**, and **Major Lazer** appeared across multiple centrality rankings
- The network is **sparse** but highly connected, forming one large connected component
- Even with low average degree, the network has a small diameter, meaning artists are only a few steps apart

---

## Predicting Popularity (Linear Regression)
A **PySpark Linear Regression** model was used to predict artist popularity using:
- Followers
- Degree
- Betweenness
- Closeness
- Eigenvector centrality
- PageRank

### Model Performance
- **Train/Test Split:** 80% / 20%
- **RMSE:** 17.62  
- **R²:** 0.3586  

### Insights
- Degree and closeness centrality had strong positive effects
- Betweenness and PageRank showed negative coefficients
- Results suggest that **direct connectivity and reachability matter more than follower count alone**

---

## Artist Clustering (K-Means)
- Applied **K-Means clustering** using the same feature set as regression
- Features were standardized using **StandardScaler**
- Tested `k = 2` to `k = 7` using the elbow method
- **Optimal k = 6**

### Findings
- One large cluster contained most low-visibility artists
- Smaller clusters captured artists with distinct collaboration or influence patterns
- Clustering revealed meaningful groupings based on network structure, not just popularity

---

## Tools & Technologies
- Python
- PySpark
- NetworkX
- Pandas
- Jupyter Notebook
- Machine Learning (Linear Regression, K-Means)

---

## Key Takeaways
- Network position plays a significant role in artist popularity
- Centrality measures provide insights beyond follower count
- Collaboration networks can help identify influential or rising artists early

---

## Potential Applications
- Music recommendation systems
- Identifying emerging artists
- Optimizing collaboration strategies for artists and labels
