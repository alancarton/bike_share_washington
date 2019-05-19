## Bike Share Washington
## ===================

The objective is to rewrite the Bike Sharing analysis using Dask data structures and ecosystem instead of plain pandas wherever possible.

Initial data was to be downloaded programmatically from Kaggle and imported into Dask dataframes, and the resulting zip and csv files are excluded from git repository

Some models were not available or caused issues when using Dask dataframes or Pandas dataframes, so where not possible, the sklearn models were employed using Dask : joblib.parallel_backend('dask')

Data Cleansing and Feature Engineering was conducted using Dask.
Where models or functions would not use Dask Dataframe, .compute() was employed but not stored as a memory object.  Was particularly required for calculation of r^2 and accuracy.

Either DASK_ML models were used, or joblib.parallel_backend('dask') for backend tasks.
Although dask-searchcv was recommended for replacement of GridSearchCV from sklearn, the module would not import due to issues with recent versions of sklearn.  sklearn had to be downloaded to version 0.20.3

Using Grid Search with XGBoost from dask caused an error - AttributeError: 'DataFrame' object has no attribute 'to_delayed'
On investigation it was suggested that it was not ready to work with Dask DataFrames (as suggested in the error) so the Dask GridSearch was run with sklearn XGBoost (which worked with fit but not with predict. Stated there was a mismatch with features - columns missing from test dataset).  This did not make sense, as test was subset of the full dataset, as was train, split after feature engineering so all columns should match.

The sklearn GridSearchCV was tried with dask XGBoost using dask joblib.parallel_backend, which worked even given above error from the dask cross validation/sklearn xgboost, so that method was continued through the rest of the exercise.

### Combined Methods

A combination was also tried using an ensemble of sklearn with RandomForestClassifier, GradientBoostingClassifier and ExtraTreesClassifier and run with Cross Validation Score from sklearn, usng the joblib.parallel_backend('dask') to use Dask, involving .compute() where necessary.

### Pipelines

Pipelines were also used in the code, but I could not find anything regarding SVC with Dask, and pipelines were suggested to use the sklearn version.  Again I involved GridsearchCV so sklearn SVC and StandardScalar were used with the sklearn GridSearchCV from sklearn, using the joblib.parallel_backend('dask') to use Dask, involving .compute() where necessary.

