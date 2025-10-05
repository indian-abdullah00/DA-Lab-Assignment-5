# Yeast Gene Expression Analysis: Dimensionality Reduction and Data Veracity Study

## 📘 Overview

This project investigates the **Yeast gene expression dataset** using **dimensionality reduction** techniques — specifically **t-SNE** and **Isomap** — to uncover hidden structures, relationships, and data quality issues.  
The primary objective is to visualize and interpret the dataset’s complex high-dimensional space (103 features and 14 binary labels) to identify **data veracity challenges** such as **label noise, outliers, and ambiguous samples**.

This work demonstrates the end-to-end data analysis pipeline from preprocessing to interpretation and serves as a foundation for advanced multi-label classification tasks.

---

## 🧩 Project Structure

### **Part A — Preprocessing and Initial Setup**
1. **Data Loading:**  
   - The dataset is loaded from an `.arff` file using `scipy.io.arff`.
   - It contains **103 features** (gene expression levels) and **14 binary functional class labels**.
   - The data is separated into:
     - `X`: Feature matrix (shape: *samples × 103*)  
     - `Y`: Label matrix (shape: *samples × 14*)

2. **Dimensionality Check:**  
   Confirm dataset size and structure to ensure proper parsing and consistency.

3. **Label Simplification for Visualization:**  
   To make visualization interpretable, the 14 binary labels are consolidated into a **single categorical label** per sample:
   - `Single: Class1` → most frequent single-label class  
   - `Multi: Class3, Class4, Class12, Class13` → most common multi-label combination  
   - `Other` → all remaining cases  

   This simplification highlights dominant biological groups and reduces visual clutter.

4. **Feature Scaling:**  
   - Applied **Standardization (Z-score scaling)** using `StandardScaler` from `sklearn`.  
   - Ensures all features contribute equally to distance-based algorithms like t-SNE and Isomap.

---

## 🌈 Part B — t-SNE Visualization and Veracity Inspection

### 1. **t-SNE Implementation**
- The **t-Distributed Stochastic Neighbor Embedding (t-SNE)** algorithm is applied to the scaled dataset for dimensionality reduction to 2D.
- Multiple **perplexity values** (`5, 25, 30, 40, 50`) are tested to observe how local vs. global relationships are preserved.
- The visualization is generated using `matplotlib` and `seaborn`.

**Chosen Parameter:**  
✅ **Perplexity = 30** was selected as the optimal setting:
- Provides a balance between capturing local detail and maintaining global structure.
- Avoids over-fragmentation (low perplexity) and over-merging (high perplexity).

---

### 2. **Veracity Analysis**

Using the t-SNE plots, several **data quality and complexity issues** are identified:

#### 🧠 a. Noisy / Ambiguous Labels
- Points from one category appearing inside another cluster (e.g., “Other” points in the multi-label cluster).  
- Indicates **labeling inconsistencies or biological ambiguity**.  
- **Impact:** Adds noise, making classification boundaries more complex and uncertain.

#### 🚨 b. Outliers
- Isolated points distant from all clusters.  
- May represent **rare gene behaviors** or **experimental errors**.  
- **Impact:** Can distort model boundaries and reduce classifier performance.

#### ⚙️ c. Hard-to-Learn Samples
- Mixed-color regions where multiple categories overlap with no clear separation.  
- Likely correspond to **genes involved in essential, shared biological processes**.  
- **Impact:** Causes inherent classification difficulty — even complex models will struggle here.

---

## 🌍 Part C — Isomap and Manifold Learning

### 1. **Isomap Implementation**
- The **Isometric Mapping (Isomap)** algorithm is applied with varying neighborhood sizes (`k = 5, 10, 15, 20, 30, 50, 100`).
- Isomap preserves **global geodesic distances** rather than local neighborhoods, offering a complementary “big-picture” perspective.

### 2. **Theoretical Comparison**

| Feature | t-SNE | Isomap |
|----------|--------|--------|
| Focus | Local neighborhoods | Global structure |
| Preserves | Local distances | Geodesic (manifold) distances |
| Best for | Cluster discovery | Manifold unfolding |
| Output | Distinct clusters | Continuous curved shapes |

### 3. **Insights from Isomap Visualization**
- Reveals a **curved, V-shaped manifold**, suggesting a **non-linear intrinsic structure** in the gene expression space.
- Indicates that specialized gene functions (`Single` and `Multi` classes) evolve along a **continuum** of expression profiles rather than being isolated.
- Supports the hypothesis that the yeast gene dataset lies on a **complex manifold**.

---

## 💡 Key Takeaways

- The Yeast dataset exhibits **multi-label complexity**, **non-linear structure**, and **label noise**.
- **t-SNE** reveals **local clusters** effectively, helping to identify ambiguous and mislabeled samples.
- **Isomap** exposes the **global geometry** and manifold curvature of the data.
- The observed **manifold complexity** explains why **non-linear models** (e.g., Kernel SVM, Random Forest, Neural Networks) are necessary for effective classification.
- Perfect accuracy is unlikely due to **inherent biological and labeling ambiguities** — a reality that the visualizations make evident.

---

## ⚙️ Technologies and Libraries Used

| Library | Purpose |
|----------|----------|
| `pandas` | Data manipulation and cleaning |
| `numpy` | Numerical computations |
| `scipy.io.arff` | Loading ARFF datasets |
| `sklearn.preprocessing.StandardScaler` | Feature standardization |
| `sklearn.manifold.TSNE` | Non-linear dimensionality reduction (local focus) |
| `sklearn.manifold.Isomap` | Manifold learning (global focus) |
| `matplotlib`, `seaborn` | Visualization and plotting |

---

## 🧭 Project Workflow Summary

```mermaid
graph TD
A[Load Yeast Dataset (.arff)] --> B[Separate Features (X) and Labels (Y)]
B --> C[Simplify Labels for Visualization]
C --> D[Scale Features (Standardization)]
D --> E[Apply t-SNE with Various Perplexities]
E --> F[Select Optimal Perplexity (30)]
F --> G[Analyze Veracity Issues: Noise, Outliers, Ambiguity]
G --> H[Apply Isomap for Global Structure]
H --> I[Compare t-SNE and Isomap Findings]
I --> J[Interpret Biological and Algorithmic Implications]
