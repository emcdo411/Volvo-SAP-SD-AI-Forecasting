![forecast_plot](https://github.com/user-attachments/assets/fc60212b-52dd-4cf8-b0ea-f700e8b8c004)

# Sweden-Relocation-Playbook

This repository documents my journey to relocate to Sweden by securing a Senior SAP SD Solution Consultant role at Volvo in Gothenburg. As part of this journey, I’ve developed three AI-driven projects to demonstrate my SAP SD expertise and AI capabilities, tailored specifically for Volvo’s SAP SD processes. These projects bridge my SAP SD experience gap by showcasing practical applications of AI in sales forecasting, pricing optimization, and delivery scheduling, all integrated with SAP SD.

## AI-Driven SAP SD Projects for Volvo

Below are three projects that leverage AI to enhance Volvo’s SAP SD processes, focusing on sales, pricing, and delivery. Each project includes detailed steps to create the necessary `.py` and `.csv` files, run the scripts, and integrate the results with SAP SD. Follow the instructions to replicate the projects on your machine.

### Project 1: Predictive Sales Forecasting for Volvo Trucks
**Goal:** Use AI to predict sales demand for Volvo trucks, optimizing inventory and production.

**Repo:** [Volvo-SAP-SD-AI-Forecasting](https://github.com/yourusername/Volvo-SAP-SD-AI-Forecasting)

#### Steps to Create and Run

1. **Create the Mock Dataset Script (`create_volvo_sales_data.py`):**
   - Create a new file named `create_volvo_sales_data.py` in your working directory (e.g., `C:\Users\YourUsername` on Windows).
   - Copy the following code into the file and save it:
     ```python
     import sys

     try:
         import pandas as pd
         import numpy as np
         from datetime import datetime, timedelta
     except ImportError as e:
         print(f"Import error: {e}")
         print("Please install the required libraries using:")
         print("pip install pandas numpy")
         sys.exit(1)

     try:
         np.random.seed(42)
         dates = pd.date_range(start="2023-01-01", end="2025-03-19", freq="D")
         quantities = np.random.randint(1, 20, size=len(dates))

         df_mock = pd.DataFrame({
             "Order_Date": dates,
             "Quantity": quantities,
             "Customer_ID": [f"CUST{i:03d}" for i in range(len(dates))],
             "Truck_Model": np.random.choice(["Volvo FH16", "Volvo VNL", "Volvo FMX"], size=len(dates)),
             "Region": np.random.choice(["Europe", "North America", "Asia"], size=len(dates))
         })

         df_mock.to_csv("volvo_sales_data.csv", index=False)
         print("Mock dataset 'volvo_sales_data.csv' created successfully!")
         print(f"Dataset contains {len(df_mock)} rows with columns: {list(df_mock.columns)}")
     except Exception as e:
         print(f"Error creating dataset: {e}")
         sys.exit(1)
     ```
   - Open a Command Prompt (Windows) or terminal (Mac/Linux) and navigate to your working directory:
     ```
     cd C:\Users\YourUsername
     ```
   - Install the required libraries:
     ```
     pip install pandas numpy
     ```
   - Run the script to generate `volvo_sales_data.csv`:
     ```
     python create_volvo_sales_data.py
     ```
   - Verify that `volvo_sales_data.csv` is created in your working directory with columns: `Order_Date`, `Quantity`, `Customer_ID`, `Truck_Model`, and `Region`.

2. **Create the Forecasting Script (`volvo_sales_forecast.py`):**
   - Create a new file named `volvo_sales_forecast.py` in the same directory.
   - Copy the following code into the file and save it:
     ```python
     try:
         from prophet import Prophet
         import pandas as pd
         import matplotlib.pyplot as plt
     except ImportError as e:
         print(f"Import error: {e}")
         print("Please install the required libraries using:")
         print("pip install prophet pandas matplotlib")
         exit(1)

     try:
         df = pd.read_csv("volvo_sales_data.csv")
         print("Dataset loaded successfully!")
     except FileNotFoundError:
         print("Error: 'volvo_sales_data.csv' not found. Run create_volvo_sales_data.py first.")
         exit(1)

     try:
         df = df[['Order_Date', 'Quantity']].rename(columns={'Order_Date': 'ds', 'Quantity': 'y'})
         df['ds'] = pd.to_datetime(df['ds'])
     except KeyError as e:
         print(f"Error: Required columns not found in dataset: {e}")
         exit(1)

     try:
         model = Prophet()
         model.fit(df)
         print("Prophet model trained successfully!")
     except Exception as e:
         print(f"Error training Prophet model: {e}")
         exit(1)

     try:
         future = model.make_future_dataframe(periods=90)
         forecast = model.predict(future)
         print("Forecast generated successfully!")
     except Exception as e:
         print(f"Error generating forecast: {e}")
         exit(1)

     try:
         plt.figure(figsize=(10, 6))
         plt.plot(df['ds'], df['y'], label='Historical Sales', marker='o', linestyle='-', alpha=0.5)
         plt.plot(forecast['ds'], forecast['yhat'], label='Forecast', color='orange')
         plt.fill_between(forecast['ds'], forecast['yhat_lower'], forecast['yhat_upper'], color='orange', alpha=0.2, label='Confidence Interval')
         plt.title("Volvo Truck Sales Forecast")
         plt.xlabel("Date")
         plt.ylabel("Quantity Sold")
         plt.legend()
         plt.grid(True)
         plt.xticks(rotation=45)
         plt.tight_layout()
         plt.savefig("forecast_plot.png")
         plt.show()
     except Exception as e:
         print(f"Error plotting forecast: {e}")

     try:
         forecast[['ds', 'yhat', 'yhat_lower', 'yhat_upper']].to_csv("volvo_sales_forecast.csv", index=False)
         print("Forecast saved to 'volvo_sales_forecast.csv'")
     except Exception as e:
         print(f"Error saving forecast: {e}")
     ```
   - Install the required libraries:
     ```
     pip install prophet pandas matplotlib
     ```
   - Run the script to generate the forecast, plot, and `volvo_sales_forecast.csv`:
     ```
     python volvo_sales_forecast.py
     ```
   - Verify that `volvo_sales_forecast.csv` and `forecast_plot.png` are created in your working directory.

3. **Document SAP SD Integration (`sap_integration.md`):**
   - Create a new file named `sap_integration.md` in the same directory.
   - Copy the following content into the file and save it:
     ```markdown
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
       ```
     ```
   - This file is for documentation purposes and will be uploaded to the GitHub repo.

#### Repo Contents
- `create_volvo_sales_data.py`: Script to generate mock sales data.
- `volvo_sales_data.csv`: Mock dataset of Volvo truck sales.
- `volvo_sales_forecast.py`: Main script for forecasting.
- `volvo_sales_forecast.csv`: Forecast output.
- `forecast_plot.png`: Visualization of the forecast.
- `sap_integration.md`: Documentation on SAP SD integration.

#### Relevance to Volvo
This project supports Volvo’s SAP SD processes by providing data-driven sales forecasts, enabling better inventory management and proactive order creation.

---

### Project 2: AI-Driven Pricing Optimization for Volvo Truck Sales
**Goal:** Use machine learning to recommend dynamic pricing for Volvo trucks, optimizing revenue.

**Repo:** [Volvo-SAP-SD-AI-Pricing](https://github.com/yourusername/Volvo-SAP-SD-AI-Pricing)

#### Steps to Create and Run

1. **Create the Mock Pricing Dataset (`create_volvo_pricing_data.py`):**
   - Create a new file named `create_volvo_pricing_data.py` in your working directory.
   - Copy the following code into the file and save it:
     ```python
     import pandas as pd
     import numpy as np

     np.random.seed(42)
     num_records = 1000
     df_pricing = pd.DataFrame({
         "Customer_ID": [f"CUST{i:03d}" for i in range(num_records)],
         "Truck_Model": np.random.choice(["Volvo FH16", "Volvo VNL", "Volvo FMX"], size=num_records),
         "Base_Price": np.random.uniform(400000, 600000, size=num_records),
         "Order_Quantity": np.random.randint(1, 50, size=num_records),
         "Market_Trend": np.random.choice(["high", "medium", "low"], size=num_records),
         "Competitor_Price": np.random.uniform(350000, 650000, size=num_records)
     })
     df_pricing.to_csv("volvo_pricing_data.csv", index=False)
     print("Mock pricing dataset 'volvo_pricing_data.csv' created successfully!")
     ```
   - Install the required libraries (if not already installed):
     ```
     pip install pandas numpy
     ```
   - Run the script to generate `volvo_pricing_data.csv`:
     ```
     python create_volvo_pricing_data.py
     ```
   - Verify that `volvo_pricing_data.csv` is created with columns: `Customer_ID`, `Truck_Model`, `Base_Price`, `Order_Quantity`, `Market_Trend`, and `Competitor_Price`.

2. **Create the Pricing Model Script (`volvo_pricing_model.py`):**
   - Create a new file named `volvo_pricing_model.py` in the same directory.
   - Copy the following code into the file and save it:
     ```python
     try:
         from sklearn.ensemble import RandomForestRegressor
         import pandas as pd
         import matplotlib.pyplot as plt
     except ImportError as e:
         print(f"Import error: {e}")
         print("Please install the required libraries using:")
         print("pip install scikit-learn pandas matplotlib")
         exit(1)

     try:
         df = pd.read_csv("volvo_pricing_data.csv")
         print("Pricing dataset loaded successfully!")
     except FileNotFoundError:
         print("Error: 'volvo_pricing_data.csv' not found. Run create_volvo_pricing_data.py first.")
         exit(1)

     df["Market_Trend"] = df["Market_Trend"].map({"high": 1, "medium": 0.5, "low": 0})
     X = df[["Order_Quantity", "Market_Trend", "Competitor_Price"]]
     y = df["Base_Price"] * (1 + np.random.uniform(0.05, 0.15, size=len(df)))

     model = RandomForestRegressor()
     model.fit(X, y)

     new_data = pd.DataFrame({
         "Order_Quantity": [50],
         "Market_Trend": [1],
         "Competitor_Price": [500000]
     })
     optimal_price = model.predict(new_data)
     print(f"Recommended Price for 50 trucks (high demand, competitor price 500,000 SEK): SEK {optimal_price[0]:,.2f}")

     plt.figure(figsize=(10, 6))
     plt.scatter(df["Base_Price"], y, label="Actual Prices", alpha=0.5)
     plt.plot([df["Base_Price"].min(), df["Base_Price"].max()], [df["Base_Price"].min(), df["Base_Price"].max()], color="red", label="Ideal")
     plt.title("Pricing Model Performance")
     plt.xlabel("Base Price (SEK)")
     plt.ylabel("Adjusted Price (SEK)")
     plt.legend()
     plt.grid(True)
     plt.savefig("pricing_plot.png")
     plt.show()
     ```
   - Install the required libraries:
     ```
     pip install scikit-learn pandas matplotlib
     ```
   - Run the script to generate the pricing model and plot:
     ```
     python volvo_pricing_model.py
     ```
   - Verify that `pricing_plot.png` is created in your working directory.

3. **Document SAP SD Integration (`sap_pricing_integration.md`):**
   - Create a new file named `sap_pricing_integration.md` in the same directory.
   - Copy the following content into the file and save it:
     ```markdown
     # SAP SD Integration for Pricing Optimization

     This project integrates with SAP SD to enhance Volvo's pricing strategy:

     - **Pricing Conditions:** Update SAP SD pricing conditions (e.g., condition type for discounts) with AI-recommended prices.
     - **Sales Orders:** Apply the recommended price to new sales orders in SAP SD.
     - **Mock SAP Integration:** Simulate updating a pricing condition in SAP SD:
       ```json
       {
         "PricingCondition": {
           "ConditionType": "ZDIS",
           "TruckModel": "Volvo FH16",
           "RecommendedPrice": 550000  // From AI model
         }
       }
       ```
     ```

#### Repo Contents
- `create_volvo_pricing_data.py`: Script to generate mock pricing data.
- `volvo_pricing_data.csv`: Mock dataset of Volvo truck pricing.
- `volvo_pricing_model.py`: Main script for pricing optimization.
- `pricing_plot.png`: Visualization of pricing performance.
- `sap_pricing_integration.md`: Documentation on SAP SD integration.

#### Relevance to Volvo
This project supports Volvo’s SAP SD processes by providing dynamic pricing recommendations, improving revenue and competitiveness.

---

### Project 3: AI-Optimized Delivery Scheduling for Volvo Trucks
**Goal:** Use AI to optimize delivery schedules for Volvo trucks, reducing costs and emissions.

**Repo:** [Volvo-SAP-SD-AI-Delivery](https://github.com/yourusername/Volvo-SAP-SD-AI-Delivery)

#### Steps to Create and Run

1. **Create the Mock Delivery Dataset (`create_volvo_delivery_data.py`):**
   - Create a new file named `create_volvo_delivery_data.py` in your working directory.
   - Copy the following code into the file and save it:
     ```python
     import pandas as pd
     import numpy as np

     np.random.seed(42)
     num_records = 50
     df_delivery = pd.DataFrame({
         "Order_ID": [f"ORD{i:03d}" for i in range(num_records)],
         "Delivery_Location": np.random.choice(["Stockholm", "Paris", "Berlin", "Madrid"], size=num_records),
         "Truck_Quantity": np.random.randint(1, 10, size=num_records),
         "Delivery_Deadline": pd.date_range(start="2025-04-01", periods=num_records, freq="D"),
         "Distance_km": np.random.randint(100, 2000, size=num_records)
     })
     df_delivery.to_csv("volvo_delivery_data.csv", index=False)
     print("Mock delivery dataset 'volvo_delivery_data.csv' created successfully!")
     ```
   - Install the required libraries (if not already installed):
     ```
     pip install pandas numpy
     ```
   - Run the script to generate `volvo_delivery_data.csv`:
     ```
     python create_volvo_delivery_data.py
     ```
   - Verify that `volvo_delivery_data.csv` is created with columns: `Order_ID`, `Delivery_Location`, `Truck_Quantity`, `Delivery_Deadline`, and `Distance_km`.

2. **Create the Delivery Optimization Script (`volvo_delivery_optimization.py`):**
   - Create a new file named `volvo_delivery_optimization.py` in the same directory.
   - Copy the following code into the file and save it:
     ```python
     try:
         from ortools.constraint_solver import routing_enums_pb2
         from ortools.constraint_solver import pywrapcp
         import pandas as pd
     except ImportError as e:
         print(f"Import error: {e}")
         print("Please install the required library using:")
         print("pip install ortools pandas")
         exit(1)

     try:
         df = pd.read_csv("volvo_delivery_data.csv")
         print("Delivery dataset loaded successfully!")
     except FileNotFoundError:
         print("Error: 'volvo_delivery_data.csv' not found. Run create_volvo_delivery_data.py first.")
         exit(1)

     locations = ["Depot"] + df["Delivery_Location"].tolist()
     distances = [
         [0, 548, 776, 878, 1234],  # Depot to Stockholm, Paris, Berlin, Madrid
         [548, 0, 1050, 960, 1350],  # Stockholm to Paris, Berlin, Madrid
         [776, 1050, 0, 1120, 980],  # Paris to Berlin, Madrid
         [878, 960, 1120, 0, 1450],  # Berlin to Madrid
         [1234, 1350, 980, 1450, 0]  # Madrid
     ]

     manager = pywrapcp.RoutingIndexManager(len(distances), 1, 0)
     routing = pywrapcp.RoutingModel(manager)

     def distance_callback(from_index, to_index):
         from_node = manager.IndexToNode(from_index)
         to_node = manager.IndexToNode(to_index)
         return distances[from_node][to_node]

     transit_callback_index = routing.RegisterTransitCallback(distance_callback)
     routing.SetArcCostEvaluatorOfAllVehicles(transit_callback_index)

     search_parameters = pywrapcp.DefaultRoutingSearchParameters()
     search_parameters.first_solution_strategy = (
         routing_enums_pb2.FirstSolutionStrategy.PATH_CHEAPEST_ARC)
     solution = routing.SolveWithParameters(search_parameters)

     if solution:
         print("Optimal Delivery Route:")
         index = routing.Start(0)
         route = []
         total_distance = 0
         while not routing.IsEnd(index):
             route.append(locations[manager.IndexToNode(index)])
             previous_index = index
             index = solution.Value(routing.NextVar(index))
             total_distance += distances[manager.IndexToNode(previous_index)][manager.IndexToNode(index)]
         route.append(locations[manager.IndexToNode(index)])
         print(" → ".join(route))
         print(f"Total Distance: {total_distance} km")
     ```
   - Install the required libraries:
     ```
     pip install ortools pandas
     ```
   - Run the script to optimize the delivery route:
     ```
     python volvo_delivery_optimization.py
     ```
   - The script will output the optimal delivery route and total distance (e.g., `Depot → Stockholm → Paris → Berlin → Madrid → Depot`).

3. **Document SAP SD Integration (`sap_delivery_integration.md`):**
   - Create a new file named `sap_delivery_integration.md` in the same directory.
   - Copy the following content into the file and save it:
     ```markdown
     # SAP SD Integration for Delivery Optimization

     This project integrates with SAP SD to enhance Volvo's delivery scheduling:

     - **Delivery Scheduling:** Update SAP SD delivery schedules with the optimized routes.
     - **Sustainability Impact:** Highlight reduced emissions, aligning with Volvo's goals.
     - **Mock SAP Integration:** Simulate updating a delivery schedule in SAP SD:
       ```json
       {
         "DeliverySchedule": {
           "OrderID": "ORD001",
           "Route": ["Depot", "Stockholm", "Paris", "Depot"],
           "TotalDistance": 2100  // From optimization
         }
       }
       ```
     ```

#### Repo Contents
- `create_volvo_delivery_data.py`: Script to generate mock delivery data.
- `volvo_delivery_data.csv`: Mock dataset of Volvo truck deliveries.
- `volvo_delivery_optimization.py`: Main script for delivery optimization.
- `sap_delivery_integration.md`: Documentation on SAP SD integration.

#### Relevance to Volvo
This project supports Volvo’s SAP SD processes by optimizing delivery schedules, reducing costs, and aligning with sustainability goals.

---

## Why These Projects Matter
- **Bridges Experience Gap:** These projects demonstrate practical SAP SD knowledge, despite my limited hands-on experience.
- **Aligns with Volvo’s Goals:** They focus on Volvo’s SAP SD processes (sales, pricing, delivery) and digital transformation priorities (sustainability, efficiency).
- **Showcases Innovation:** Combining AI with SAP SD positions me as a forward-thinking candidate, adding value beyond traditional SAP skills.

## How to Use This Repository
1. Clone this repository to your local machine:
   ```
   git clone https://github.com/yourusername/Sweden-Relocation-Playbook.git
   ```
2. Navigate to the project directories (`Volvo-SAP-SD-AI-Forecasting`, `Volvo-SAP-SD-AI-Pricing`, `Volvo-SAP-SD-AI-Delivery`) and follow the steps in each section to create and run the scripts.
3. Review the `.md` files for details on SAP SD integration.

## Next Steps
- **Share on LinkedIn:** I’ve shared these projects on LinkedIn to increase visibility with Volvo recruiters and SAP professionals.
- **Apply to Volvo:** These projects are highlighted in my resume and cover letter to demonstrate my value as a candidate.
- **Engage with Communities:** I’m sharing these projects on the SAP Community Network (community.sap.com) and connecting with Volvo SAP consultants.

## Contact
Feel free to reach out to me on [LinkedIn](https://www.linkedin.com/in/yourusername) to discuss these projects or my relocation journey to Sweden!
```

---

### How to Add This to Your GitHub Repository
1. **Create or Update the `Sweden-Relocation-Playbook` Repository:**
   - If you haven’t created the repository yet, go to GitHub, create a new repository named `Sweden-Relocation-Playbook`, and initialize it with a README.
   - If the repository already exists, open it in your GitHub account.

2. **Add the README Content:**
   - Open the `README.md` file in your repository (or create it if it doesn’t exist).
   - Copy the entire Markdown content above and paste it into the `README.md` file.
   - Replace `yourusername` with your actual GitHub username in all links (e.g., `https://github.com/johndoe/Sweden-Relocation-Playbook`).
   - Save and commit the changes.

3. **Create the Individual Project Repositories:**
   - Create three separate repositories on GitHub: `Volvo-SAP-SD-AI-Forecasting`, `Volvo-SAP-SD-AI-Pricing`, and `Volvo-SAP-SD-AI-Delivery`.
   - For each repository, upload the files listed in the “Repo Contents” section of the respective project (e.g., `.py`, `.csv`, `.png`, and `.md` files).
   - Create a `README.md` file in each project repo with the content provided in the earlier steps (e.g., the “Repo Contents,” “How to Run,” and “Relevance to Volvo” sections).

4. **Verify the Links:**
   - Ensure the links in the `Sweden-Relocation-Playbook` README point to the correct repositories (e.g., `https://github.com/yourusername/Volvo-SAP-SD-AI-Forecasting`).
   - Test the links to confirm they work after uploading the project repos.

---

### Why This README is GitHub-Worthy
- **Professional Structure:** The README is well-organized with clear sections, headings, and bullet points, making it easy to navigate.
- **Detailed Instructions:** It provides step-by-step guidance to create and run each script, including how to handle `.py` and `.csv` files, avoiding past mistakes like running Markdown files or missing files.
- **Focus on Volvo:** Each project highlights its relevance to Volvo’s SAP SD processes, aligning with the job role and Volvo’s goals (e.g., sustainability, digital transformation).
- **Showcases Skills:** The projects demonstrate your ability to combine AI with SAP SD, positioning you as an innovative candidate.
- **Engagement:** It includes calls to action (e.g., LinkedIn sharing, community engagement) to increase visibility.

---

### Next Steps
- **Share on LinkedIn:** Post about your completed projects: “Excited to share my AI-driven SAP SD projects for Volvo! Predictive sales forecasting, pricing optimization, and delivery scheduling, all integrated with SAP SD. Check them out: [link to Sweden-Relocation-Playbook repo]. #SAP #AI #Volvo #Tech”
- **Apply to Volvo:** Use this README and the project repos in your application to highlight your skills and value.
- **Engage with Communities:** Share the projects on the SAP Community Network and connect with Volvo SAP consultants.

Would you like to enhance any of these projects further (e.g., add more features to the delivery optimization), or move on to another part of your relocation journey? 😊 Let me know!
