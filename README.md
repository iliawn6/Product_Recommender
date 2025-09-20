# 🎯 Deep Learning Recommender System (Amazon Dataset)

This project implements a **deep learning–based recommender system** that predicts how much a user would like a given product.  
By learning patterns in historical interactions between users and items, the model recommends products a user has not yet purchased but is likely to rate highly.
While trained on the **Amazon Electronics** dataset, the approach is domain-agnostic and works equally well for movies, music, books, or general e-commerce.

&nbsp;  
## 📂 Dataset

**Fields:**
- **UserID** — unique user identifier  
- **ProductID** — unique product identifier  
- **Rating** — user’s rating of the product  
- **Timestamp** — when the rating was registered  


&nbsp;  
## ⚖️ Class Imbalance & Oversampling

Real-world ratings are rarely uniform (e.g., far more positive than neutral/negative). Training directly on such data can bias the model toward majority classes and harm recommendation quality.

This project uses **random oversampling** (via `imbalanced-learn`) with a **custom sampling strategy** to rebalance selected underrepresented rating classes. In practice:

- We **increase the presence** of specific classes (e.g., mid/low ratings) to a target count proportional to their original size (e.g., ×2, ×2.5, ×3).  
- Oversampling is applied **only to the training split**, preserving the natural distribution in validation/test.  
- This leads to **more stable training** and **crisper decision boundaries**, improving precision on relevant items.

> Why not just upsample everything to the majority?  
> Targeted oversampling avoids exploding dataset size and preserves a realistic label mix, while still correcting harmful skew.


&nbsp;  
## 🧠 Approach (High-Level)

The system follows a **Neural Collaborative Filtering** style:

- **User & Item Embeddings** capture latent preferences and item attributes.  
- A **user–item interaction** layer models affinity (with learned user and item biases).  
- A **sigmoid output** produces a continuous relevance score in [0, 1].

This design balances **expressiveness** (via embeddings) and **scalability** (simple interaction head) for large recommendation spaces.


&nbsp;  
## 🧪 Training & Evaluation

- **Optimizer:** Adam (LR 1e-3)  
- **Loss:** Binary cross-entropy (aligned with [0,1] target)  
- **Metric:** **Precision** on held-out data  
- **Result:** **84% precision**

This indicates the model is effective at surfacing items users are likely to appreciate.


&nbsp;  
## 🔎 Producing Recommendations

When generating recommendations for a specific user, the system first identifies the set of products that the user has not interacted with yet. Each of these candidate products is then scored by the model, which outputs a value between 0 and 1 representing the likelihood of the user giving a high rating. The candidates are ranked according to their predicted scores, and the top results are presented as recommendations. A utility function is provided to automate this process, making it straightforward to request, for example, the top five most promising products for any given user.


&nbsp;  
✨ With this project, the aim is not only to recommend products but to demonstrate how thoughtful design choices like embedding-based modeling and class balancing can make a significant impact on system quality. May it inspire further innovations in building smarter, fairer, and more useful recommendation engines.

Happy Recommending! 🚀
