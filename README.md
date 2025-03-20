![forecast_plot](https://github.com/user-attachments/assets/fc60212b-52dd-4cf8-b0ea-f700e8b8c004)

# SAP SD Integration for Predictive Sales Forecasting

This project integrates with SAP SD to enhance Volvo's sales planning and order management:

- **Sales Planning:** The forecast (`volvo_sales_forecast.csv`) can be imported into SAP SD to adjust sales targets. For example, if the forecast predicts high demand in Europe, Volvo can pre-allocate inventory to European warehouses.
- **Order Management:** Use the forecast to proactively create sales orders in SAP SD for high-demand regions or truck models.
- **Mock SAP Integration:** Simulate updating a sales order in SAP SD with forecasted quantities:
  ```json
  {
    "SalesOrder": {
      "OrderID": "SO12345",
      "CustomerID": "CUST001",
      "TruckModel": "Volvo FH16",
      "Quantity": 15,  // From forecast
      "Region": "Europe",
      "OrderDate": "2025-06-01"
    }
  }

  
- **Verify README.md Links to `sap_integration.md`:**
Open the `README.md` in your `Volvo-SAP-SD-AI-Forecasting` repo (or create it if you haven’t already) and ensure it references `sap_integration.md`. Here’s the `README.md` content for reference:
```markdown
# Predictive Sales Forecasting for Volvo Trucks

This project uses AI to predict sales demand for Volvo trucks, helping optimize inventory and production. The forecast integrates with SAP SD to enhance sales planning and order management.

## Features
- Generates a 90-day sales forecast using Facebook Prophet.
- Visualizes historical sales and forecast with confidence intervals.
- Simulates integration with SAP SD for sales order updates.

## Files
- `create_volvo_sales_data.py`: Script to generate mock sales data.
- `volvo_sales_data.csv`: Mock dataset of Volvo truck sales.
- `volvo_sales_forecast.py`: Main script for forecasting.
- `volvo_sales_forecast.csv`: Forecast output.
- `forecast_plot.png`: Visualization of the forecast.
- `sap_integration.md`: Documentation on SAP SD integration.

## How to Run
1. Install dependencies: `pip install pandas numpy prophet matplotlib`
2. Run `create_volvo_sales_data.py` to generate the dataset: `python create_volvo_sales_data.py`
3. Run `volvo_sales_forecast.py` to generate the forecast and plot: `python volvo_sales_forecast.py`

## Relevance to Volvo
This project supports Volvo's SAP SD processes by providing data-driven sales forecasts, enabling better inventory management and proactive order creation.
