# Scikit-Learn (sklearn) Cheat Sheet

## Importing Libraries
```python
import numpy as np  # For numerical operations
import pandas as pd  # For data manipulation
from sklearn.model_selection import train_test_split  # For splitting data
from sklearn.preprocessing import StandardScaler  # For standardizing features
from sklearn.linear_model import LinearRegression  # For linear regression
from sklearn.metrics import mean_squared_error, accuracy_score, confusion_matrix  # For evaluating models
from sklearn.datasets import load_iris  # For loading sample datasets
```

## Loading Data
### From a CSV file
```python
df = pd.read_csv('file.csv')  # Load data from a CSV file
X = df.drop('target_column', axis=1)  # Features
y = df['target_column']  # Target variable
```

### From sklearn datasets
```python
data = load_iris()  # Load iris dataset
X = data.data  # Features
y = data.target  # Target variable
```

## Splitting Data
```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)  # Split data into train and test sets
```

## Preprocessing Data
### Standardization
```python
scaler = StandardScaler()  # Initialize scaler
X_train_scaled = scaler.fit_transform(X_train)  # Fit and transform training data
X_test_scaled = scaler.transform(X_test)  # Transform test data
```

### One-Hot Encoding
```python
from sklearn.preprocessing import OneHotEncoder  # For one-hot encoding

encoder = OneHotEncoder(sparse=False)  # Initialize encoder
y_train_encoded = encoder.fit_transform(y_train.reshape(-1, 1))  # Fit and transform training labels
y_test_encoded = encoder.transform(y_test.reshape(-1, 1))  # Transform test labels
```

## Model Training
### Linear Regression
```python
model = LinearRegression()  # Initialize linear regression model
model.fit(X_train, y_train)  # Train the model
```

### Logistic Regression
```python
from sklearn.linear_model import LogisticRegression  # For logistic regression

model = LogisticRegression()  # Initialize logistic regression model
model.fit(X_train, y_train)  # Train the model
```

### Decision Trees
```python
from sklearn.tree import DecisionTreeClassifier  # For decision trees

model = DecisionTreeClassifier()  # Initialize decision tree classifier
model.fit(X_train, y_train)  # Train the model
```

### Random Forests
```python
from sklearn.ensemble import RandomForestClassifier  # For random forests

model = RandomForestClassifier()  # Initialize random forest classifier
model.fit(X_train, y_train)  # Train the model
```

### Support Vector Machines (SVM)
```python
from sklearn.svm import SVC  # For support vector machines

model = SVC()  # Initialize support vector classifier
model.fit(X_train, y_train)  # Train the model
```

## Model Prediction
```python
y_pred = model.predict(X_test)  # Make predictions on test data
```

## Model Evaluation
### Regression Metrics
```python
mse = mean_squared_error(y_test, y_pred)  # Calculate mean squared error
rmse = np.sqrt(mse)  # Calculate root mean squared error
print(f'MSE: {mse}, RMSE: {rmse}')  # Print MSE and RMSE
```

### Classification Metrics
```python
accuracy = accuracy_score(y_test, y_pred)  # Calculate accuracy
conf_matrix = confusion_matrix(y_test, y_pred)  # Calculate confusion matrix
print(f'Accuracy: {accuracy}')  # Print accuracy
print(f'Confusion Matrix:\n{conf_matrix}')  # Print confusion matrix
```

## Cross-Validation
```python
from sklearn.model_selection import cross_val_score  # For cross-validation

scores = cross_val_score(model, X, y, cv=5)  # Perform 5-fold cross-validation
print(f'Cross-Validation Scores: {scores}')  # Print cross-validation scores
print(f'Mean CV Score: {scores.mean()}')  # Print mean cross-validation score
```

## Hyperparameter Tuning
### Grid Search
```python
from sklearn.model_selection import GridSearchCV  # For grid search

param_grid = {'C': [0.1, 1, 10, 100], 'kernel': ['linear', 'rbf']}  # Define parameter grid
grid = GridSearchCV(SVC(), param_grid, refit=True, verbose=2)  # Initialize grid search
grid.fit(X_train, y_train)  # Perform grid search

print(f'Best Parameters: {grid.best_params_}')  # Print best parameters
```

### Random Search
```python
from sklearn.model_selection import RandomizedSearchCV  # For random search

param_dist = {'C': [0.1, 1, 10, 100], 'kernel': ['linear', 'rbf']}  # Define parameter distribution
random_search = RandomizedSearchCV(SVC(), param_distributions=param_dist, n_iter=10, random_state=42)  # Initialize random search
random_search.fit(X_train, y_train)  # Perform random search

print(f'Best Parameters: {random_search.best_params_}')  # Print best parameters
```

## Saving and Loading Models
### Using joblib
```python
import joblib  # For saving/loading models

joblib.dump(model, 'model.pkl')  # Save the model to a file
model = joblib.load('model.pkl')  # Load the model from a file
```

### Using pickle
```python
import pickle  # For saving/loading models

with open('model.pkl', 'wb') as f:  # Open file to write
    pickle.dump(model, f)  # Save the model to the file

with open('model.pkl', 'rb') as f:  # Open file to read
    model = pickle.load(f)  # Load the model from the file
```

## Pipelines
```python
from sklearn.pipeline import Pipeline  # For creating pipelines

pipeline = Pipeline([
    ('scaler', StandardScaler()),  # Standardize features
    ('clf', SVC())  # Classify using SVM
])

pipeline.fit(X_train, y_train)  # Train the pipeline
y_pred = pipeline.predict(X_test)  # Make predictions using the pipeline
```

## Feature Selection
### Using SelectKBest
```python
from sklearn.feature_selection import SelectKBest, chi2  # For feature selection

selector = SelectKBest(score_func=chi2, k=10)  # Select top 10 features based on chi-squared test
X_new = selector.fit_transform(X, y)  # Fit and transform the features
```

### Using Recursive Feature Elimination (RFE)
```python
from sklearn.feature_selection import RFE  # For recursive feature elimination

selector = RFE(estimator=LogisticRegression(), n_features_to_select=5)  # Select top 5 features using RFE
selector.fit(X_train, y_train)  # Fit the selector
```
