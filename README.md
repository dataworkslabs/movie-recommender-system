Movie Recommender System

A personalized movie recommendation engine built from scratch in R, using User-to-User Collaborative Filtering on a real survey dataset of 16 critics and 6 movies.
This project was completed in two phases — first implementing a non-personalized Global Baseline Estimate, then building a fully personalized collaborative filtering system and evaluating whether personalization actually improved accuracy.
What It Does

Predicts what rating a user would give to a movie they haven't seen
Generates a ranked recommendation list per user
Evaluates prediction accuracy using leave-one-out cross-validation

How It Works
The algorithm follows four steps:

Center ratings — subtract each user's average to remove personal scale bias (a generous rater and a harsh rater can still have identical taste)
Compute similarity — measure Pearson correlation between every pair of users based on their co-rated movies
Find neighbors — for a target user–movie pair, identify the most similar users who have rated that movie
Predict — estimate the rating as the user's average plus a weighted average of neighbors' deviations, where more similar neighbors get more weight

When the model can't find valid neighbors (due to sparse data), it falls back to the Global Baseline Estimate.
Results
ModelRMSEGlobal Baseline Estimate1.10Collaborative Filtering1.02
The personalized model achieved a 7.7% improvement over the non-personalized baseline across 52 evaluated ratings using leave-one-out cross-validation.
Repository Structure
├── Personalized_Recommender_Hamer.qmd    # Main analysis (Quarto source)
├── Personalized_Recommender_Hamer.html   # Rendered report with all output
├── MovieRatings.xlsx                      # Survey dataset
├── Global_Baseline_Estimate.qmd          # Phase 1: non-personalized baseline
├── Global_Baseline_Estimate.html         # Phase 1: rendered report
└── README.md
Key Formulas
Centered Rating:
## Key Formulas

**Centered Rating:**

$$\tilde{r}(u, i) = r(u, i) - \bar{r}(u)$$

**Pearson Similarity:**

$$sim(u, v) = \frac{\sum \tilde{r}(u,i) \cdot \tilde{r}(v,i)}{\sqrt{\sum \tilde{r}(u,i)^2} \cdot \sqrt{\sum \tilde{r}(v,i)^2}}$$

**Predicted Rating:**

$$\hat{r}(u, i) = \bar{r}(u) + \frac{\sum sim(u,v) \cdot \tilde{r}(v,i)}{\sum |sim(u,v)|}$$
Minimum 2 co-rated movies required to compute similarity between users
Positive similarity only — users with opposite taste are excluded as neighbors to avoid adding noise in a small dataset
k = 5 neighbors maximum per prediction
Global Baseline fallback for cold-start users with insufficient rating history

Limitations

Dataset is small (16 users × 6 movies) with 42% sparsity, which limits the reliability of similarity estimates
Predictions can fall outside the 1–5 rating scale
The 7.7% improvement, while consistent, would likely be more pronounced on a larger dataset

Tools

Language: R
Document format: Quarto (.qmd)
Packages: readxl (data loading only — all algorithms implemented from scratch)

Author
Mark Hamer
