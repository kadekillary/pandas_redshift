## Synopsis

This package is designed to make it easier to get data from redshift into a pandas DataFrame and vice versa.
The pandas_redshift package only supports python3.

## Installation

```python
pip install pandas-redshift
```

## Code Example
```python
import pandas_redshift as pr
```

Connect to redshift. If port is not supplied set to amazon default 5439

```python
pr.connect_to_redshift(dbname = <dbname>,
                        host = <host>,
                        port = <port>,
                        user = <user>,
                        password = <password>)
```

Query redshift and return a pandas DataFrame.

```python
data = pr.redshift_to_pandas('select * from gawronski.nba_shots_log')

data.ix[0:5,0:5]

GAME_ID1                   MATCHUP LOCATION  W  FINAL_MARGIN
0  21400899  MAR 04, 2015 - CHA @ BKN        A  W            24
1  21400899  MAR 04, 2015 - CHA @ BKN        A  W            24
2  21400899  MAR 04, 2015 - CHA @ BKN        A  W            24
3  21400899  MAR 04, 2015 - CHA @ BKN        A  W            24
4  21400899  MAR 04, 2015 - CHA @ BKN        A  W            24
5  21400899  MAR 04, 2015 - CHA @ BKN        A  W            24
```

Write a pandas DataFrame to redshift. Requires access to an S3 bucket and previously having connected to redshift (with pr.connect_to_redshift). If the table exists IT WILL BE DROPPED and then the pandas DataFrame will be put in it's place.

```python
# Connect to S3
pr.connect_to_s3(aws_access_key_id = <aws_access_key_id>,
                aws_secret_access_key = <aws_secret_access_key>,
                bucket = <bucket>,
                subdirectory = <subdirectory>)

# Write the DataFrame to S3
pr.pandas_to_redshift(data_frame = data,
                        redshift_table_name = 'gawronski.nba_shots_log')

```

Other options:

```python
pr.pandas_to_redshift(data_frame,
                        redshift_table_name,
                        # Defaults:
                        column_data_types = None, # A list of column data types. If not supplied all columns will default to varchar(256)
                        index = False,
                        save_local = False, # If set to True a csv from the data frame will save in the current directory
                        delimiter = ',',
                        quotechar = '"',
                        dateformat = 'auto',
                        timeformat = 'auto')
```
Redshift data types: http://docs.aws.amazon.com/redshift/latest/dg/c_Supported_data_types.html

Finally close the cursor, commit and close the database connection, and remove variables from the environment.

```python
pr.close_up_shop()
```
