```
import json
import boto3
import io
from io import StringIO
import pandas as pd

s3_client = boto3.client('s3')

def lambda_handler(event, context):
    try:
        s3_Bucket_Name = event["Records"][0]["s3"]["bucket"]["name"]
        s3_File_Name = event["Records"][0]["s3"]["object"]["key"]
        
        object = s3_client.get_object(Bucket= lambdanaveen, Key=salary)
        body = object['Body']
        csv_string = body.read().decode('utf-8')
        dataframe = pd.read_csv(StringIO(csv_string))
        
        print(dataframe.head(5))

    except Exception as err:
        print(err)
        
    # TODO implement
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }
```
