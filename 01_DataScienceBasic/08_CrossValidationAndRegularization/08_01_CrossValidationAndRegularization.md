
Cross Validation and Regulatization is the method to present the model overfitting 


![[image/Pasted image 20250602144146.png]]

the following method is aimed to mitigrate the validation error 

# 1 K-Fold Croess Validation 


![[image/Pasted image 20250602144447.png]]


validaition set 从 training set 中拿到 


这个方法的缺点   计算量大   每个 parameter * k (个fold) 都要做一次计算 


# 2 Constraining Model Parameters


使用 Regularization 


if without Regularization:   


## 2.1 L1 Regularization

"least absolute shrinkage and selection operator



selection operator:  select which feature to use through setting the value of  thema parameter 




## 2.2 L1 Regularization 


this solution exists even if 𝕏 is not full rank: full rank 

### 2.2.1 💡 What Does "Full Rank" Mean?

A matrix is **full rank** if **its rank is equal to the number of its columns** (for a tall matrix) or the number of its rows (for a wide matrix). Specifically:

- For an m×nm \times nm×n matrix X\mathbf{X}X, the **rank** is the number of **linearly independent columns** (or rows).
    
- If X\mathbf{X}X has **full column rank**, then **none of the features (columns) are linearly dependent** on the others.
    

---

### 2.2.2 🤔 Why Is Full Rank Important in Regression?

In **ordinary least squares (OLS)**, we compute:

θ^OLS=(XTX)−1XTY\hat{\theta}_{\text{OLS}} = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{Y}θ^OLS​=(XTX)−1XTY

This requires inverting XTX\mathbf{X}^T \mathbf{X}XTX. For the inverse to exist:

- XTX\mathbf{X}^T \mathbf{X}XTX must be **non-singular** (i.e., invertible).
    
- This only happens if X\mathbf{X}X has **full column rank**.
    

---

### 2.2.3 ✅ Why L2 Regularization Helps

With **L2 regularization** (Ridge Regression), we use:

θ^ridge=(XTX+nλI)−1XTY\hat{\theta}_{\text{ridge}} = (\mathbf{X}^T \mathbf{X} + n\lambda \mathbf{I})^{-1} \mathbf{X}^T \mathbf{Y}θ^ridge​=(XTX+nλI)−1XTY

Here, adding nλIn\lambda \mathbf{I}nλI (where λ>0\lambda > 0λ>0) **ensures** that the matrix is invertible **even if X\mathbf{X}X is not full rank**. This is one reason why L2 regularization is so powerful—it stabilizes the solution in the presence of **multicollinearity** or **redundant features**.

---

### 2.2.4 🔁 Summary:

- **Full rank** = all columns are linearly independent.
    
- Not full rank → XTX\mathbf{X}^T \mathbf{X}XTX is not invertible → OLS breaks.
    
- L2 regularization (Ridge) adds a term that **guarantees invertibility**, making it robust to multicollinearity or redundant features.

