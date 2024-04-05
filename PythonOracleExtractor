import oracledb
import pandas as pd

connection = oracledb.connect(
   user="",
   password="",
   dsn="")

print("Successfully connected to Oracle Database")

cursor = connection.cursor()

schema_name = ''  # Change this to the schema name you want

table_names_query = f"SELECT table_name FROM all_tables WHERE owner = '{schema_name}'"
cursor.execute(table_names_query)

table_names = [row[0] for row in cursor.fetchall()]

# Create a list to store table, column, and specification data
table_column_spec_data = []

for table_name in table_names:
   column_spec_query = f"""
   SELECT column_name, data_type, data_length, nullable
   FROM all_tab_columns
   WHERE owner = '{schema_name}' AND table_name = '{table_name}'
   """
   cursor.execute(column_spec_query)
   column_spec_data = cursor.fetchall()

   for column_data in column_spec_data:
       column_name, data_type, data_length, nullable = column_data
       table_column_spec_data.append((table_name, column_name, data_type, data_length, nullable))

# Close the cursor and connection
cursor.close()
connection.close()

# Create a DataFrame from the combined data
data_df = pd.DataFrame(table_column_spec_data, columns=['Table Name', 'Column Name', 'Data Type', 'Data Length', 'Nullable'])

# Export DataFrame to an Excel file
excel_file = 'name.xlsx'
data_df.to_excel(excel_file, index=False)

print("Data exported to", excel_file)
