# Seoul-Bike-Renting-Synthetic-Data-Generation
Synthetic data was generated using the original dataset from UCI ML Repository (https://archive.ics.uci.edu/dataset/560/seoul+bike+sharing+demand)

**Synthetic Data Creation and Evaluation**
In this task, you will work with a synthetic data generation tool and evaluate the fidelity, utility and privacy aspects of the synthetic data; to keep computation times reasonable, you shall work with tabular data. The idea is that you use existing tools, and mainly develop the boilerplate code around using these tools (dataset loading, spitting, model training, calling evaluation tools, ....).

The exercise thus consists of roughly the following steps:

_Pick a tabular dataset from a public repository; ideally, the dataset contains some sensitive/personal attributes (age, income, education, diagnosis, ....). Overall, your dataset does not need to be too large, anything from 1k samples up should be fine.
(we focus on tabular data due to computational costs/runtime, interpretability of the results, and the meaningfulness of fidelity & privacy evaluation; if you would want to work with some other data modality, contact me, we can discuss if that is feasible)_

We picked the Seoul-Bike-Renting dataset (having 14 features) from the UCI ML Repository (https://archive.ics.uci.edu/dataset/560/seoul+bike+sharing+demand). 

_Chose a data synthetisation framework; while there are many frameworks around, we can recommend the (Synthetic Data Vault)[https://github.com/sdv-dev/SDV] (SDV) as a very comprehensive tool including many different techniques (their original Gaussian Copula, but also CTGAN, and implementations of Bayesian Networks, ...) , gretel.ai; synthpop offers decision trees (primary implementation is in R), DataSynthesizer offers Bayesian Networks. You can find a curated list e.g. at https://github.com/joofio/awesome-data-synthesis_

The SDV framework is utilized and we used the TVAESynthesizer technique with enforce_min_max_values=True, enforce_rounding=True, and epochs=2000. 


_Prepare your data for evaluation; basically, you shall have a test set available for testing classifiers_

Only 80% of the original data was utilized to generate synthetic data; for simplicity reasons, just created a dataset with the same number of samples as the original dataset. The 20% remaining was kept to evaluate the classifier models trained separately on the original and its corresponding synthetic data.


_Perform a fidelity evaluation of the synthetic dataset by comparing data characteristics to the original training data; you can utilise, e.g. the SDMetric from the SDV https://github.com/sdv-dev/SDMetrics; select the most interesting aspects from the evaluation for your report._

| Comparison Metrix | Value | Explaination|
| --- | --- | --- |
| `Contingency Similarity` | 0.37787950383933844 | As the best here is the value that is more close to 1.0. Our value is low which indicates there can be any reason among the following:
- Independence: When the two categorical variables are independent of each other, meaning the occurrence of one variable does not affect the occurrence of the other. For example, the colour of a car and the type of music played in it might have low contingency similarity because they are unlikely to be related.
- Randomness: In situations where the occurrences of the variables are purely random and not influenced by each other, the contingency similarity would be low. For example, the outcomes of two unrelated dice rolls would have low contingency similarity.
- Weak Association: Even if there is some association between the variables, if the association is weak, the contingency similarity would be low. For instance, in a survey where respondents are asked about their favourite colour and favourite animal, there might be some association (e.g., people who like blue might also like dogs), but if the association is weak, the contingency similarity would be low.
- Small Sample Size: With a small sample size, the contingency similarity might be low due to insufficient data to establish a meaningful association between the variables. It's important to consider the statistical power and significance level when interpreting contingency tables.
| `Correlation Similarity` | 0.9814272609470664 |As the best here is the value that is more close to 1.0. Our correlation similarity index is high which indicates:
- Strong Linear Relationship: From the skewness property comparison, it is evident that both synthetic and original data share a lot of resemblance.
- Tightly Clustered Data Points: When the data points in a scatter plot are tightly clustered around a straight line (either positively or negatively sloped), it indicates a strong linear relationship and high correlation similarity. `In our case, we can see that we have synthetically generated data which is near to the original data in terms of its distribution`.
| `TVComplement` | 0.6822209179514293 | This value suggests that a Total Variation distance complement of 0.68 would suggest that `there is some difference between the distributions, but they share a notable portion of their shapes. It's an indication of similarity, though not complete identity, between the distributions`.

_Perform a utility evaluation of the synthetic dataset by training one classifier model, once on the original training data and once on the synthetic data; then evaluate both models on the test data and compare their performance. Use a fast, shallow, but still powerful model (e.g. SVM, RF, Boosting algorithms, ....)_

R <sup>2</sup> performance is as follows:


| Types of Data | _Liner Regression_ | _Decision Tree_ | _Random Forest_ | _LGBM_ |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| _Origional Data_  | 0.5992307630988627| 0.7833303632422974| 0.8798205269762169| 0.8877639365966321|
| _Synthetic Data_  | 0.5566763675130999| 0.31099428512224725| 0.6704767677084573| 0.711977783868252|

_Perform a privacy evaluation of the synthetic dataset, by using a privacy evaluation tool. You can use e.g. Anonymeter (https://github.com/statice/anonymeter), and focus on metrics that can be computed from the synthetic data directly. There are other tools around, but many of them provide attack-based evaluation (e.g. membership inference attacks), which are computationally expensive. Again, select the most interesting aspects from the evaluation for your report._

| Types of Data |
| ------------- | 
| _Inference Evaluator_  | 
| _SinglingOut Evaluator_  |
| _Multivariat_  |
