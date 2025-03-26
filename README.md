# Product Recommendation System  

This project implements a **machine learning-based product recommendation system** to provide personalized product suggestions to users based on their browsing and purchase history. The system uses **collaborative filtering** and **content-based filtering** techniques to analyze user behavior and generate relevant recommendations, enhancing the shopping experience and boosting sales for e-commerce businesses.  

## 📁 Dataset  

The dataset used contains Amazon user ratings for electronic products. To ensure fairness, products and users are represented by unique identifiers rather than names or other potentially biased information.  

- **Dataset Link:** [Amazon Electronics Rating Dataset](https://www.kaggle.com/datasets/vibivij/amazon-electronics-rating-datasetrecommendation/download?datasetVersionNumber=1)  
- **Alternative Sources:** [Amazon Review Datasets](https://jmcauley.ucsd.edu/data/amazon/)  

## 🚀 Approach  

### **1️⃣ Rank-Based Product Recommendation**  
**Objective:**  
- Recommend the most popular products with the highest number of ratings.  
- Target new customers with trending items.  
- Address the **[Cold Start Problem](ColdStartProblem.md)** (when new users/items lack sufficient interaction data).  

**Output:**  
- Top 5 products with a minimum of 50 or 100 ratings/interactions.  

**Method:**  
1. Calculate the **average rating** for each product.  
2. Count the **total number of ratings** per product.  
3. Create a DataFrame sorted by average ratings.  
4. Define a function to retrieve the top *n* products with a specified minimum interaction threshold.  

---  

### **2️⃣ Similarity-Based Collaborative Filtering**  
**Objective:**  
- Provide **personalized recommendations** based on user behavior similarity.  

**Output:**  
- Top 5 products liked by similar users.  

**Method:**  
1. Convert `user_id` from object type to integer (0 to 1539) for easier processing.  
2. **Find similar users:**  
   - Compute **cosine similarity** between the target user and all others.  
   - Sort similarity scores and extract the most similar users.  
   - Exclude the original user and return the closest matches.  
3. **Generate recommendations:**  
   - Identify products interacted with by similar users but **not** by the target user.  
   - Return the top *n* recommended products.  

---  

### **3️⃣ Model-Based Collaborative Filtering (SVD)**  
**Objective:**  
- Provide **personalized recommendations** while handling data sparsity and scalability.  

**Output:**  
- Top 5 predicted product recommendations for a given user.  

**Method:**  
1. Convert the ratings matrix into a **Compressed Sparse Row (CSR)** format for efficiency.  
2. Apply **Singular Value Decomposition (SVD)** to reduce dimensionality (50 latent features).  
3. Predict ratings by reconstructing the matrix using SVD components (`U`, `Σ`, `Vᵀ`).  
4. **Recommendation Function:**  
   - Compare actual vs. predicted ratings.  
   - Filter out already-rated products.  
   - Sort by predicted ratings and return top suggestions.  
5. **Model Evaluation:**  
   - Compute **Root Mean Squared Error (RMSE)** between actual and predicted ratings.  
   - (RMSE is obtained by setting `squared=False` in `mean_squared_error`).  

