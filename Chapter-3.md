# Data Pre-Processing

Data transformations including reducing the skewness or outliers can
lead to huge prediction improvements.

At a high level,

- The need for data processing is determined by the type of model being used. For
example, tree based models are insensitive to predictor data characteristics.

- Sometimes using combinations of predictors be more effective than using individual
values.

## Individual predictors

### Centering and Scaling

- Subtract by mean and divide by standard deviation.

- Most models benefit from the predictors being on a common scale.

- Tree-based models are not affected again.

### Skewness

Right-skewed distribution has a large no. of points on the left side of the distribution.

_Thumb rule_: In a histogram of the data, if ratio of highest value to lowest value > 20, then the data has
significant skewness.

Formally,

![Skewness](/images/chapter-3/skewness.png "Skewness Expression")

Replacing the data with log, square root or inverse may help reduce the skew. A
more standard approach is to perform the Box-Cox transformation as shown below:

![Box-Cox Transform](/images/chapter-4/box_cox.png "Box-Cox transformation")

Maximum likelihood estimation can be utilized for tuning the transform order
independently for each predictor data **that contain value greater than zero**.

## Multiple predictors

### Resolving Outliers

If a model is sensitive to outliers, one approach is the *spatial sign*.

![Spatial Sign](/images/chapter-4/spatial_sign.png "Spatial Sign")

- Transformation happens on the entire group, not at variable level.

- Scale and center before applying spatial sign.

- Removing predictor variables after this transform is difficult.


### PCA

- **Unsupervised** feature extraction

- Linear transformations

- Transform skewed predictors then center and scaling before PCA

## Dealing with Missing Values

- Is the pattern of missing data related to the outcome? **Informative missingness**
can induce *significant bias* in the model.

- People are more compelled to rate products when they have strong opinions. In this case,
data is more likely to be polarized with only few values in the middle of the rating range.

- **Censored data** is different from missing data. Exact value is missing but something is
known about the value for e.g., out of range.

- If below limit of detection, limit can be used as replacement value or a **random value between 0 and the limit**.

### Imputation

- If number of predictors affected by missing values are small, an exploratory analysis
to find out the predictor with highest correlation and using regression to impute is a good approach.

- **K-nearest neighbor imputation**: One disadvantage is that entire training set is required
every time a value needs to be imputed.

## Degenerate Predictors

First, the number of unique values in the data must be small. Second, the ratio of the most common frequency to the second most common frequency reflects the imbalance in the frequencies.

**Conditions for Removal of Predictor:**

1. Fraction of unique values to sample size is low (< 10%) and

2. Ratio of frequency of the most prevalent value to the frequency of the second
most prevalent value is large (~ 20).

## Between-Predictor Correlations

Linear models can utilize statistic called Variance Inflation factor
to identify predictors impacted by multicollinearity.


**Heuristic 2-dimensional approach**:

*Strategy* is to remove the minimum number of predictors to ensure all pairwise
correlations are below a pre-defined threshold.

1. Calculate correlation matrix of predictors.

2. Determine two predictors associated with the largest absolute pairwise corr.

3. Determine average corr between A and other variables. Same for B.

4. Remove the one with the higher average corr.

5. Repeat 2-4 until no absolute pairwise correlations are above threshold.

## Adding Predictors

For classification models, complex combinations of the data can be added using the
following approach:

1. Calculate the class centroids: centers of predicted data for each class.

2. For each predictor, compute the distance to each class centroid.

3. Add these new features to the model.
