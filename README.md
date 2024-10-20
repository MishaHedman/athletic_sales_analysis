# athletic_sales_analysis

# Import Libraries and Dependencies
import pandas as pd

athletic_sales_2020_path = ("Resources/athletic_sales_2020.csv")
athletic_sales_2021_path = ("Resources/athletic_sales_2021.csv")

# Read the CSV files into DataFrames.
athletic_sales_2020 = pd.read_csv(athletic_sales_2020_path)
athletic_sales_2021 = pd.read_csv(athletic_sales_2021_path)

# Display the 2020 sales DataFrame
athletic_sales_2020.head()

# Display the 2021 sales DataFrame
athletic_sales_2021.head()

# Check the 2020 sales data types.
athletic_sales_2020.dtypes

# Check the 2021 sales data types.
athletic_sales_2021.dtypes

# Combine the 2020 and 2021 sales DataFrames on the rows and reset the index.
Sales_2020_2021 = pd.concat([athletic_sales_2020, athletic_sales_2021], axis='rows').reset_index (drop=True)
Sales_2020_2021.head()

# Check if any values are null.
Sales_2020_2021.isnull().sum()

# Check the data type of each column
Sales_2020_2021.dtypes

# Convert the "invoice_date" to a datetime datatype
Sales_2020_2021['invoice_date'] = pd.to_datetime(Sales_2020_2021['invoice_date'])

# Confirm that the "invoice_date" data type has been changed.
Sales_2020_2021.dtypes

# Show the number products sold for region, state, and city.
# Rename the sum to "Total_Products_Sold".
Total_Products_Sold = Sales_2020_2021.groupby(['region', 'state', 'city']).agg(Total_Products_Sold=('units_sold', 'sum'))
Total_Products_Sold = Total_Products_Sold.sort_values(by='Total_Products_Sold',ascending=False)
# Show the top 5 results.
Total_Products_Sold.head()

# Show the number products sold for region, state, and city.
pivot_table_2020_2021=Sales_2020_2021.pivot_table(values='units_sold',
                                  index=['region', 'state', 'city'],
                                  aggfunc='sum')

# Rename the "units_sold" column to "Total_Products_Sold"
pivot_table_2020_2021=pivot_table_2020_2021.rename(columns={'units_sold' : 'Total_Products_Sold'})
pivot_table_2020_2021=pivot_table_2020_2021.sort_values(by='Total_Products_Sold',ascending=False)

# Show the top 5 results.
pivot_table_2020_2021.head()

# Show the total sales for the products sold for each region, state, and city.
# Rename the "total_sales" column to "Total Sales"
Total_Sales = Sales_2020_2021.groupby(['region', 'state', 'city']).agg(Total_Sales=('total_sales', 'sum'))
Total_Sales = Total_Sales.sort_values(by='Total_Sales',ascending=False)

# Show the top 5 results.
Total_Sales.head()

# Show the total sales for the products sold for each region, state, and city.
pivot_total_sales=Sales_2020_2021.pivot_table(values='total_sales',
                                  index=['region', 'state', 'city'],
                                  aggfunc='sum')

# Optional: Rename the "total_sales" column to "Total Sales"
pivot_total_sales=pivot_total_sales.rename(columns={'total_sales' : 'Total Sales'})
pivot_total_sales=pivot_total_sales.sort_values(by='Total Sales',ascending=False)

# Show the top 5 results.
pivot_total_sales.head()

# Show the total sales for the products sold for each retailer, region, state, and city.
# Rename the "total_sales" column to "Total Sales"
Total_Product_Sales = Sales_2020_2021.groupby(['retailer', 'region', 'state', 'city']).agg(Total_Sales=('total_sales', 'sum'))
Total_Product_Sales = Total_Product_Sales.sort_values(by='Total_Sales',ascending=False)

# Show the top 5 results.
Total_Product_Sales.head()

# Show the total sales for the products sold for each retailer, region, state, and city.
pivot_total_sales=Sales_2020_2021.pivot_table(values='total_sales',
                                  index=['retailer', 'region', 'state', 'city'],
                                  aggfunc='sum')

# Optional: Rename the "total_sales" column to "Total Sales"
pivot_total_sales=pivot_total_sales.rename(columns={'total_sales' : 'Total Sales'})
pivot_total_sales=pivot_total_sales.sort_values(by='Total Sales',ascending=False)

# Show the top 5 results.
pivot_total_sales.head()

Sales_2020_2021.columns

# Filter the sales data to get the women's athletic footwear sales data.
filtered_data = Sales_2020_2021.loc[Sales_2020_2021['product'] == "Women's Athletic Footwear"]
filtered_data.head(10)

# Show the total number of women's athletic footwear sold for each retailer, region, state, and city.
# Rename the "units_sold" column to "Womens_Footwear_Units_Sold"
Womens_Footwear_Units_Sold = filtered_data.groupby(['retailer', 'region', 'state', 'city'])\
    ['units_sold'].agg(Womens_Footwear_Units_Sold=('sum'))

Womens_Footwear_Units_Sold = Womens_Footwear_Units_Sold.sort_values(by=(["Womens_Footwear_Units_Sold"]),ascending=False)

# Show the top 5 results.
Womens_Footwear_Units_Sold.head()

# Show the total number of women's athletic footwear sold for each retailer, region, state, and city.
pivot_total_womens_footwear=filtered_data.pivot_table(
                                values='units_sold',
                                index=['retailer', 'region', 'state', 'city'],
                                aggfunc='sum')

# Rename the "units_sold" column to "Womens_Footwear_Units_Sold"
pivot_total_womens_footwear=pivot_total_womens_footwear.rename(columns={'units_sold' : "Womens_Footwear_Units_Sold"})
pivot_total_womens_footwear=pivot_total_womens_footwear.sort_values(by="Womens_Footwear_Units_Sold",ascending=False)

# Show the top 5 results.
pivot_total_womens_footwear.head()

# Create a pivot table with the 'invoice_date' column is the index, and the "total_sales" as the values.
pivot_sales_day=filtered_data.pivot_table(values='total_sales',
                                  index=['invoice_date'],
                                  aggfunc='sum')


# Optional: Rename the "total_sales" column to "Total Sales"
pivot_sales_day=pivot_sales_day.rename(columns={'total_sales' : 'Total Sales'})

# Show the table.
pivot_sales_day.head(10)

pivot_sales_day=filtered_data.pivot_table(values='total_sales',
                                  index=['invoice_date'],
                                  aggfunc='sum')
pivot_sales_day.rename(columns={'total_sales' : 'Total Sales'}, inplace=True)
daily_sales = pivot_sales_day.resample('D').sum()

# Sort the resampled pivot table in descending order on "Total Sales".
daily_sales=daily_sales.sort_values(by='Total Sales', ascending=False)
daily_sales.head(10)

# Resample the pivot table into weekly bins, and get the total sales for each week.
pivot_sales_day=filtered_data.pivot_table(values='total_sales',
                                  index=['invoice_date'],
                                  aggfunc='sum')
pivot_sales_day.rename(columns={'total_sales' : 'Total Sales'}, inplace=True)

weekly_sales = pivot_sales_day.resample('W').sum()

# Sort the resampled pivot table in descending order on "Total Sales".
weekly_sales=weekly_sales.sort_values(by='Total Sales', ascending=False)
weekly_sales.head(10)

