---
title: '"Trading System - Low Level System Design (Data Processor)"'
draft: 
tags: []
---
### High-Level Flow:

**1. Data Ingestion & Storage (Airflow + S3 + MinIO)**:
- **S3**: The data (CSV files) from the BSE website will be ingested and stored in the **S3** bucket, which serves as the source of truth.
- **MinIO**: We’ll use MinIO (self-hosted) for feature storage.
- **Airflow**: Airflow will be used to automate the processes of downloading, converting CSV to Parquet, organizing it into the structured folder format, and moving it to MinIO for later retrieval.

**2. Feature Engineering Pipeline (Airflow + Feast + MLflow)**:
- **Feature Store (Feast)**: The processed Parquet data will be ingested into the **Feature Store**, where features like returns, moving averages, and volatility will be calculated and stored for later model training or analysis.
- **MLflow**: MLflow will be used to log experiments, track model performance, and store metadata for features.
- **Airflow Tasks**: Each step will be orchestrated by Airflow to ensure automation and reproducibility.

**3. Data Serving (MinIO + Feast)**:
- **MinIO**: Serve the feature data to ML services, trading systems, or analytics systems, ensuring that it is easily accessible and fast.
- **Feast**: Ensure that features (historical and real-time) are served to machine learning models, analytics, or used in clustering, depending on the needs of the application.

Now, let's visualize this with a high-level diagram:

### High-Level Diagram:

```plaintext
+-----------------------+                +------------------+              +-------------------+         
|  BSE Website          |                |    S3 Bucket     |              |    Airflow        |
| (CSV Data)            |   --->         | (Data Storage)   |   --->       | (Automation of    |
|                       |   Download     |                  |   Process    |  Data Ingestion,  |
|                       |                |                  |   & Feature  |  Conversion,      |
+-----------------------+                +------------------+              |  Feature Gen.)    |
                                                        |                     +-------------------+         
                                                        v                                   
                                               +---------------+                          
                                               |  MinIO (Feature|                          
                                               |  Storage)      |                          
                                               +---------------+                           
                                                         |                              
                                                         v                               
                                               +---------------+                      
                                               |   Feast (Feature|   
                                               |   Store)        |       
                                               +-----------------+              
                                                         |                                 
                                                         v                                 
                                             +----------------------+
                                             | ML Services or       |
                                             | Analytics Services   |
                                             | (Clustering, Models, |
                                             | Analysis)            |
                                             +----------------------+

```

### Description:
1. **Data Ingestion**: The BSE website provides data at 8 PM, which is scraped, downloaded, and stored in S3.
2. **Data Transformation**: A process (either through Airflow or a script) converts CSV to Parquet, organizes it into structured folders in S3.
3. **Feature Engineering**: The features are calculated (e.g., returns, volatility, moving averages) using a feature engineering pipeline. These features are stored in **MinIO** and added to the **Feast Feature Store**.
4. **Data Serving**: ML services or analytics tools retrieve features from the **Feature Store** (Feast) for use in models, clustering, or analysis.

# Feature Store

### What is a Feature Store?
A **Feature Store** is a centralized repository for storing, managing, and serving features that are used for training and inference in machine learning models. It helps solve the problem of feature duplication and ensures consistency across both training and production pipelines.

### Key Responsibilities of a Feature Store:
1. **Feature Engineering**: Features are derived from raw data (e.g., OHLCV data) and stored in a structured format.
2. **Feature Serving**: The Feature Store makes features available to both training and inference processes (e.g., for ML model training and real-time predictions).
3. **Feature Management**: It ensures that features are tracked, versioned, and organized correctly.
4. **Consistency**: Features used during training are the same ones used during inference (avoiding the "training/production skew").
5. **Scaling**: It handles both historical (batch) and real-time (streaming) features.

### Step-by-Step Process for Feature Engineering and Storing in Feature Store

#### 1. **Feature Creation**
   - **Input Data**: Raw data (OHLCV, intraday) is first ingested into the system (e.g., in Parquet format in MinIO).
   - **Feature Engineering**:
     - **Technical Indicators**: Moving averages, volatility, returns, momentum, etc.
     - **Rolling Statistics**: Standard deviation, moving average windows, etc.
     - **Lag Features**: Previous day's data, n-day lag features.
   - **Feature Engineering Tasks**: 
     - These are orchestrated using **Airflow**. Airflow tasks can schedule and run the feature engineering jobs, making sure that all features are calculated and stored correctly.

#### 2. **Feature Versioning and Tracking**
   - **Version Control**: Features can be versioned to ensure that historical and new features are managed correctly. This helps in scenarios where you need to re-train models with older versions of the features.
   - **Metadata**: Information such as the type of feature (technical, statistical), the time window used, and the source of the feature is recorded. This can be stored in a metadata database or tracked via tools like **MLflow** or **Feast** itself.

#### 3. **Storage in Feature Store**
   - **Feast** is the tool we're using as the **Feature Store**. It allows us to manage and serve features in a centralized way.
   - **Feature Groups**: Organize features by category (e.g., `price_features`, `volatility_features`).
   - **Serving**: Features are stored in **Feast** and served to downstream systems (ML models, analytics).

   **Feast's Workflow**:
   1. **Define Feature Sets**: Define the features you want to store in the Feature Store (e.g., daily returns, moving averages, volatility).
   2. **Register Features**: Use Feast's Python SDK to register the features in the Feature Store (e.g., `feast.Feature`, `feast.FeatureView`).
   3. **Batch and Stream Serving**: Feast handles both batch and stream serving. For daily features, batch serving is sufficient. However, real-time features (e.g., intraday data) can be served using stream-based serving.
   4. **Feature Lookup**: Models or other systems (trading system, ML model) can look up the features they need via a simple API (RESTful, gRPC, etc.).

#### 4. **Serving Real-Time Features**
   - **Real-Time Serving**: For real-time models (e.g., intraday prediction), we can serve features like intraday returns, current price, and volatility from Feast in real-time via stream serving.
   - **Historical Features**: Historical features (e.g., past returns, historical moving averages) are served in batch mode (daily).

### Managing New Features

As the system evolves, you'll want to introduce new features into the pipeline. Here’s how you can handle new features:

1. **Feature Design & Testing**:
   - **Define**: New features are defined as part of the feature engineering process (e.g., "3-day moving average").
   - **Testing**: New features can be tested offline first (backtesting or training models offline) to evaluate their usefulness. You can run an **Airflow** task to calculate these new features and store them in the Feature Store.
   
2. **Adding New Features to the Feature Store**:
   - **Airflow Task**: Create a new **Airflow task** to calculate and ingest the new feature into the Feature Store.
   - **Feature Registration in Feast**: Use Feast's `FeatureView` to define and register the new feature. Here's a quick example of how to register a feature:
   
   ```python
   from feast import Feature, FeatureView, FileSource
   from feast.types import Float32
   
   # Define the feature source (e.g., Parquet data from MinIO)
   feature_source = FileSource(
       name="ohlcv_data",
       path="s3://your_bucket/path/to/parquet",
       event_timestamp_column="timestamp",
   )

   # Define the new feature: 3-day moving average
   moving_avg_feature = Feature(
       name="moving_average_3d",
       dtype=Float32,
   )

   # Register the feature in Feast
   feature_view = FeatureView(
       name="price_features",
       entities=["security_id"],
       features=[moving_avg_feature],
       batch_source=feature_source,
       ttl=3600,  # Time to live
   )
   ```

3. **Feature Validation**:
   - **Validation**: Once a new feature is added, validate it in both **training** and **production** environments. Ensure that the feature is available when the model or system needs it.
   - **Monitoring**: Set up monitoring to track the feature’s usage in production and check for issues (e.g., missing values, feature drift).

4. **Versioning and Experiment Tracking**:
   - **MLflow**: Track and log any new feature transformations and the models that use them. Keep a version history of the features used for model training.
   - **Feature Store Versioning**: Use Feast’s built-in versioning mechanism to ensure that each feature version is tracked.

#### 5. **Serving Features for Models/Analytics**
   - **Batch Serving**: For daily features (e.g., daily returns), features are served in batches by Feast for model training and backtesting.
   - **Real-Time Serving**: For intraday data, features are served in real-time, allowing live trading systems to get updated features.

### Low-Level API Contracts for Feature Store

1. **Ingest Data (Parquet to Feature Store)**:
   - **POST /feature-store/ingest**: An API endpoint for ingesting data into the feature store (data from MinIO).
   
   **Request**:
   ```json
   {
       "feature_view": "price_features",
       "data_source": "s3://path/to/parquet",
       "features": ["moving_average_3d", "volatility"],
       "timestamp": "2024-11-15T20:00:00Z"
   }
   ```
   
   **Response**:
   ```json
   {
       "status": "success",
       "message": "Data successfully ingested into the feature store"
   }
   ```

2. **Feature Lookup**:
   - **GET /feature-store/lookup**: API to retrieve a feature for a specific entity (security) at a given timestamp.
   
   **Request**:
   ```json
   {
       "entity_id": "AAPL",
       "feature_name": "moving_average_3d",
       "timestamp": "2024-11-15T20:00:00Z"
   }
   ```
   
   **Response**:
   ```json
   {
       "entity_id": "AAPL",
       "feature_name": "moving_average_3d",
       "value": 150.75,
       "timestamp": "2024-11-15T20:00:00Z"
   }
   ```

### Summary of the Process:
1. Data is ingested from S3 to the feature store (MinIO).
2. Features are created using predefined transformations and stored in **Feast**.
3. New features are registered and versioned.
4. Features are served to downstream systems for model training or inference.
5. Feature validation and monitoring ensure data consistency and reliability.

# How things are computed ? 
Sure! Let’s focus on **Feature Store** (using Feast), **Airflow**, and how the system will handle **new data** (like moving averages and other features) while ignoring other components like MinIO and S3. We’ll also clarify how **MLflow** integrates with Feast.

### **Goal:**
We want to update the **Feature Store** with new data on a regular basis (e.g., daily updates for moving averages and other calculated features), and Airflow will orchestrate this workflow.

### High-Level Flow:

1. **Data Ingestion** (from external sources like BSE website) happens periodically (e.g., daily or intraday).
2. **Feature Engineering** (moving averages, returns, etc.) is triggered by Airflow after the data is ingested.
3. **Feast** (the Feature Store) is used to store and manage the features, making them accessible for real-time and batch processing.
4. **MLflow** helps with logging the feature engineering process and managing metadata for model training.

### Step-by-Step Workflow:

#### 1. **Airflow Orchestrates Feature Engineering**
Airflow will periodically trigger the feature engineering process:
- **Task 1**: Airflow fetches the new data (OHLCV, intraday) for each stock.
- **Task 2**: Airflow triggers a Python operator (or Spark operator, if needed) to run feature engineering on the new data (e.g., calculate moving averages, returns, volatility).

#### 2. **Feature Engineering on New Data (Moving Averages)**
Let’s say the daily new data has arrived (for a stock, it could be OHLCV data for a given day). Now we want to calculate a **moving average** for that stock.

- **Moving Average Calculation**: 
  - Suppose we're calculating a **5-day Simple Moving Average (SMA)**.
  - **Step 1**: Get the last 5 days of closing prices (or any other relevant price data).
  - **Step 2**: Compute the average of those 5 closing prices.
  - **Step 3**: Store this calculated feature for later use in models or analytics.

- **For New Data**:
  - Each time new data comes in (e.g., at 8 PM), Airflow will:
    1. Trigger a Python task to recalculate the moving average using the most recent data.
    2. Update the moving average for each stock based on the most recent 5 days (or whatever window is defined).
    3. The calculation could also include other features like volatility or returns over time.

#### 3. **Ingest New Features into Feast (Feature Store)**

Once the moving averages (or other features) are calculated:
- **Airflow triggers a PythonOperator** that interacts with **Feast** to store these new features.
- **Feast** provides a convenient API to register features and store them with proper metadata.
- **Feature storage in Feast** is typically done in **"Feature Views"**, which define a set of features for a particular entity (in this case, a stock) and a specific timeframe (e.g., daily, weekly).
  - You can think of each stock as an "entity" and the features (like moving averages) as attributes of that entity.
  
**Feast API Usage**:
- **Feast Client** (`feast.Client`) will be used in your PythonOperator to push features into the Feature Store.

  ```python
  import feast
  from feast import Feature, FeatureView, Entity
  from feast.data_source import FileSource
  import pandas as pd

  # Load new data (e.g., calculated moving average)
  new_data = pd.DataFrame({
      "stock_id": ["AAPL", "GOOG", "AMZN"],  # example stocks
      "moving_average_5": [150, 2500, 3100],  # new calculated features
      "timestamp": ["2024-11-17", "2024-11-17", "2024-11-17"]  # timestamp of data
  })

  # Define the Feature Store structure in Feast
  feature_store = feast.Client()

  # Create or update the Feature View for moving averages
  stock_entity = Entity(name="stock_id", value_type=feast.ValueType.STRING)
  moving_avg_feature = Feature(name="moving_average_5", dtype=feast.ValueType.FLOAT)

  feature_view = FeatureView(
      name="moving_averages",
      entities=[stock_entity],
      features=[moving_avg_feature],
      ttl=3600,
      batch_source=FileSource(path="s3://my-bucket/data", event_timestamp_column="timestamp")
  )

  # Register or update the feature view
  feature_store.apply(feature_view)

  # Ingest new feature data into Feast
  feature_store.materialize(new_data)
  ```

In this code:
- **FeatureView**: Defines the features like the moving average for a stock. It also links to a data source (like a CSV file or S3).
- **Materialize**: This method pushes the new data into the Feature Store, making it available for future use.

#### 4. **Using MLflow for Feature Tracking and Model Logging**

While **Feast** handles the feature storage, **MLflow** is used to track the entire feature engineering process and maintain metadata.

- **MLflow** is responsible for:
  1. Logging feature transformations: For example, you can log the process of calculating the moving average or other features in MLflow.
  2. Tracking hyperparameters and parameters used in the feature engineering steps.
  3. Storing model artifacts and configurations if you're building machine learning models.
  4. Versioning of features and models.

- **MLflow Integration with Feast**:
  - You can store the feature engineering scripts as part of the MLflow project.
  - When training a model using Feast features, you can log the model training process in **MLflow**.
  
Here’s how you can integrate MLflow for logging:

```python
import mlflow
import mlflow.sklearn

# Start MLflow run
with mlflow.start_run():
    # Log feature engineering parameters
    mlflow.log_param("moving_average_window", 5)
    
    # Log the feature engineering process (e.g., moving average)
    mlflow.log_metric("avg_moving_average_5", new_data["moving_average_5"].mean())
    
    # If you are training a model, log it with MLflow
    # mlflow.sklearn.log_model(model, "model")
    
    # Log the feature engineering script (optional)
    mlflow.log_artifact("path_to_feature_script.py")

    # Once feature generation is done, update the Feature Store with Feast
    feature_store.materialize(new_data)
```

### Summary of Key Points:
1. **Airflow** orchestrates the entire process: Data ingestion, feature engineering, and storing features in the Feature Store.
2. **Feast** is used for managing features and making them accessible for real-time and batch serving.
3. **MLflow** is used to track the feature engineering process, log metadata, and monitor models (if needed).
