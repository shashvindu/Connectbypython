# Connectbypython
import json
import pyodbc  # for SQL Server (Azure SQL, DB2), Teradata
import pymysql  # for MySQL
from teradatasql import connect  # for Teradata
from azure.storage.blob import BlobServiceClient  # for Azure Blob Storage
from azure.identity import ClientSecretCredential  # for Azure ADLS

# Load credentials from JSON file
with open('credentials.json') as json_file:
    credentials = json.load(json_file)

# Connecting to Teradata
td_conn = connect(
    host=credentials['teradata']['host'],
    user=credentials['teradata']['user'],
    password=credentials['teradata']['password']
)

# Connecting to Azure SQL (using pyodbc)
azure_sql_conn = pyodbc.connect(
    'Driver={ODBC Driver 17 for SQL Server};'
    f'Server={credentials['azure_sql']['server']};'
    f'Database={credentials['azure_sql']['database']};'
    f'Uid={credentials['azure_sql']['user']};'
    f'Pwd={credentials['azure_sql']['password']};'
)

# Connecting to DB2 (using pyodbc)
db2_conn = pyodbc.connect(
    f'DRIVER={{IBM DB2 ODBC DRIVER}};'
    f'DATABASE={credentials['db2']['database']};'
    f'HOSTNAME={credentials['db2']['host']};'
    f'PORT={credentials['db2']['port']};'
    f'PROTOCOL=TCPIP;'
    f'UID={credentials['db2']['user']};'
    f'PWD={credentials['db2']['password']};'
)

# Connecting to Oracle
# You would use a library like cx_Oracle for this

# Connecting to MySQL
mysql_conn = pymysql.connect(
    host=credentials['mysql']['host'],
    user=credentials['mysql']['user'],
    password=credentials['mysql']['password'],
    database=credentials['mysql']['database']
)

# Connecting to Azure Blob Storage
blob_service_client = BlobServiceClient.from_connection_string(
    credentials['azure_blob']['connection_string']
)

# Connecting to Azure ADLS using Service Principal
tenant_id = credentials['azure_adls']['tenant_id']
client_id = credentials['azure_adls']['client_id']
client_secret = credentials['azure_adls']['client_secret']
credential = ClientSecretCredential(tenant_id, client_id, client_secret)

# Now you can use these connections/credentials as needed for your tasks
