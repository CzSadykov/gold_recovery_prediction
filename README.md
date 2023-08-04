**Problem situation**: on the request for our customer we need to optimize production in order to successfully launch an industrial enterprise while avoiding losses.

**Goal**: prepare a prototype of a machine learning model for the Tsifra company. The model should predict the recovery rate of gold from an ore.

**Task**: Predict the efficiency of rough concentrate (```rougher.output.recovery```) and final concentrate (```final.output.recovery```) of gold, using cross-validation to achieve the lowest value of the **sMAPE** metric

**Data**: mining and refining parameters.

**Datasets**:

* ```gold_recovery_full_new.csv``` — initial data;
* ```gold_recovery_train_new.csv``` — training sample;
* ```gold_recovery_test_new.csv``` — test set.

## Conclusions 

So that's what we've done:

### Processed data

* Successfully uploaded and analyzed the data, confirmed that the performance indicator was calculated correctly.
* Selected redundant and irrelevant features for the model. For example, the technical parameters of a flotation plant.
* For current features, gaps have been replaced based on available historical data.


### Analyzed data: concentrations of metals and sizes of granules

* It was found that from the moment the feedstock arrives, the concentration of gold increases with each stage of purification, the concentration of silver increases at the flotation stage, then decreases (to values close to the initial ones) at the stage of primary purification, and at the final stage it decreases to a minimum. As for lead, its concentration increases at the stages of flotation and primary purification, to values relatively close to the final ones.
* Distributions of average concentrations after flotation begin to skew to the left.
* In addition, with each purification step, the concentration of gold relative to other metals increases. The concentration of silver drops most of all, lead in the final remains in a larger proportion. On average, the concentration of metals does not strongly depend on time, which indicates the stability of the technological process and raw materials.
* The distributions of the initial raw material granules in the training and test sets are practically the same.
* We also saw anomalous data in the analysis of total concentrations and removed objects with a total concentration of less than 35 (831 objects).

### Trained and tested models

* Trained linear regression, decision tree and random forest. At the same time, cross-validation on 5 blocks was used to assess the quality of the model. **Random forest turned out to be the best model based on training results** – for both target features, the values of the sMAPE metric were higher for it than for other models.
* To predict the enrichment efficiency at the flotation stage, a model with a number of trees of 11 and a depth of 5 was chosen.
* To predict the enrichment efficiency at the final stage, a model with a number of trees of 26 and a depth of 2 was chosen.
* Achieved the final sMAPE metric, which takes into account the metrics of both stages, at the level of **12.57%**. This is **1.08%** higher than average predictions, which means that further work is needed. 


### Further work:

We shall:

* Make additional inquiries into data to find out the reason of metrics being higher than the average baseline even in models with built-in regularization (i.e. Random Forest);
* If everything is ok with data, engineer new features to improve the models' quality;
* Try models using gradient boosting (LightGBM, CatBoost). 
