## Hands-on projects and case studies for Data Science & Analytics using Python on the AWS platform. These projects will help you understand how to leverage AWS services for various data science tasks.

---

## Hands-on Projects and Case Studies for Data Science & Analytics using Python on AWS Platform

### 1. Setting Up Your AWS Environment

Before starting with the projects, set up your AWS environment:

1. **Create an AWS Account**: Sign up at [AWS](https://aws.amazon.com/).
2. **Set Up IAM Users and Roles**: Create IAM users with appropriate permissions.
3. **Install AWS CLI**: [Install and configure the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).
4. **Set Up Python Environment**: Install Python and required libraries like `boto3`, `sagemaker`, and `pandas`.

### 2. Project 1: Data Ingestion and Storage

#### Objective
Ingest data from various sources and store it in Amazon S3.

#### Steps
1. **Create an S3 Bucket**:
    ```bash
    aws s3 mb s3://my-data-bucket
    ```

2. **Ingest Data**:
    - Ingest data from local files:
      ```python
      import boto3

      s3 = boto3.client('s3')
      s3.upload_file('local_file.csv', 'my-data-bucket', 'data/local_file.csv')
      ```

    - Ingest data from web APIs:
      ```python
      import requests
      import pandas as pd

      url = 'https://api.example.com/data'
      response = requests.get(url)
      data = response.json()

      df = pd.DataFrame(data)
      df.to_csv('data.csv', index=False)

      s3.upload_file('data.csv', 'my-data-bucket', 'data/api_data.csv')
      ```

3. **Verify Data in S3**:
    ```bash
    aws s3 ls s3://my-data-bucket/data/
    ```

### 3. Project 2: Data Processing and Transformation

#### Objective
Process and transform raw data using AWS Glue and store the transformed data in Amazon Redshift.

#### Steps
1. **Set Up AWS Glue**:
    - Create a Glue Database:
      ```python
      import boto3

      glue = boto3.client('glue')
      glue.create_database(DatabaseInput={'Name': 'my_database'})
      ```

    - Create a Glue Crawler to catalog data:
      ```python
      glue.create_crawler(
          Name='my-crawler',
          Role='AWSGlueServiceRole',
          DatabaseName='my_database',
          Targets={'S3Targets': [{'Path': 's3://my-data-bucket/data/'}]}
      )
      glue.start_crawler(Name='my-crawler')
      ```

2. **Set Up Amazon Redshift**:
    - Create a Redshift Cluster.
    - Connect to Redshift and create tables.

3. **Create an ETL Job in AWS Glue**:
    - Write a Glue ETL script:
      ```python
      import sys
      from awsglue.transforms import *
      from awsglue.utils import getResolvedOptions
      from pyspark.context import SparkContext
      from awsglue.context import GlueContext
      from awsglue.job import Job

      args = getResolvedOptions(sys.argv, ['JOB_NAME'])
      sc = SparkContext()
      glueContext = GlueContext(sc)
      spark = glueContext.spark_session
      job = Job(glueContext)
      job.init(args['JOB_NAME'], args)

      # Read data from S3
      datasource0 = glueContext.create_dynamic_frame.from_catalog(database = "my_database", table_name = "data")

      # Transform data
      applymapping1 = ApplyMapping.apply(frame = datasource0, mappings = [("col1", "string", "col1", "string"), ("col2", "int", "col2", "int")])

      # Write data to Redshift
      glueContext.write_dynamic_frame.from_options(frame = applymapping1, connection_type = "redshift", connection_options = {"url": "jdbc:redshift://my-redshift-cluster:5439/mydb", "user": "myuser", "password": "mypassword", "dbtable": "public.mytable"}, transformation_ctx = "datasink4")

      job.commit()
      ```

    - Run the ETL job in AWS Glue.

### 4. Project 3: Data Analysis and Visualization

#### Objective
Analyze and visualize data using Amazon Athena and Amazon QuickSight.

#### Steps
1. **Set Up Amazon Athena**:
    - Create a table in Athena pointing to the S3 data.
      ```sql
      CREATE EXTERNAL TABLE IF NOT EXISTS mydatabase.mytable (
        col1 STRING,
        col2 INT
      )
      ROW FORMAT DELIMITED
      FIELDS TERMINATED BY ','
      STORED AS TEXTFILE
      LOCATION 's3://my-data-bucket/data/';
      ```

    - Query data in Athena:
      ```sql
      SELECT col1, COUNT(*) FROM mydatabase.mytable GROUP BY col1;
      ```

2. **Set Up Amazon QuickSight**:
    - Create a dataset in QuickSight:
      ```bash
      aws quicksight create-data-set --aws-account-id 123456789012 --data-set-id mydataset --name "My Dataset" --physical-table-map file://physical-table-map.json
      ```

    - Create visualizations and dashboards in QuickSight.

### 5. Project 4: Machine Learning Model Deployment

#### Objective
Build, train, and deploy a machine learning model using Amazon SageMaker.

#### Steps
1. **Set Up Amazon SageMaker**:
    - Create a SageMaker notebook instance.

2. **Prepare Data**:
    ```python
    import pandas as pd

    # Load data from S3
    data = pd.read_csv('s3://my-data-bucket/data/processed_data.csv')

    # Split data into train and test sets
    from sklearn.model_selection import train_test_split

    train, test = train_test_split(data, test_size=0.2)
    train.to_csv('train.csv', index=False)
    test.to_csv('test.csv', index=False)
    ```

3. **Train a Model**:
    ```python
    import sagemaker
    from sagemaker import get_execution_role
    from sagemaker.sklearn.estimator import SKLearn

    role = get_execution_role()

    # Define an estimator
    sklearn_estimator = SKLearn(
        entry_point='train.py',
        role=role,
        instance_count=1,
        instance_type='ml.m5.large',
        framework_version='0.23-1'
    )

    # Train the model
    sklearn_estimator.fit({'train': 's3://my-data-bucket/data/train.csv'})
    ```

4. **Deploy the Model**:
    ```python
    predictor = sklearn_estimator.deploy(initial_instance_count=1, instance_type='ml.m5.large')

    # Make predictions
    predictions = predictor.predict(test.drop(columns=['target']))
    ```

### 6. Case Studies

#### Case Study 1: E-commerce Recommendation System
- **Objective**: Build a recommendation system for an e-commerce platform using collaborative filtering.
- **AWS Services**: Amazon SageMaker, Amazon Redshift, Amazon S3.
- **Steps**:
  1. Ingest user purchase data into Amazon Redshift.
  2. Use Amazon SageMaker to build and train a collaborative filtering model.
  3. Deploy the model and generate recommendations.

#### Case Study 2: Sentiment Analysis on Social Media
- **Objective**: Perform sentiment analysis on social media posts to gauge public opinion about a product.
- **AWS Services**: Amazon Comprehend, Amazon S3, Amazon QuickSight.
- **Steps**:
  1. Collect social media posts using APIs and store them in Amazon S3.
  2. Use Amazon Comprehend to perform sentiment analysis on the posts.
  3. Visualize the results using Amazon QuickSight.

#### Case Study 3: Fraud Detection in Financial Transactions
- **Objective**: Detect fraudulent transactions using machine learning.
- **AWS Services**: Amazon SageMaker, Amazon Athena, AWS Glue.
- **Steps**:
  1. Ingest transaction data into Amazon S3.
  2. Use AWS Glue to preprocess and transform the data.
  3. Train a machine learning model in Amazon SageMaker to detect fraud.
  4. Deploy the model and monitor its performance.

### 7. Conclusion
These hands-on projects and case studies provide a comprehensive overview of how to use AWS services for data science and analytics. By following these examples, you can leverage the power of AWS to build, train, and deploy data-driven solutions.

---

This guide is a starting point for exploring data science and analytics on AWS. For more detailed information, you can explore the [AWS Documentation](https://docs.aws.amazon.com/), [AWS Training and Certification](https://aws.amazon.com/training/), and [AWS Machine Learning Blog](https://aws.amazon.com/blogs/machine-learning/).
