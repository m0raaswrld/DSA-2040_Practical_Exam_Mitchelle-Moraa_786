`Project README: End-of-Semester Data Mining and Business Intelligence Project`

**Project Overview and Submission Details**


Project Title	Comprehensive Data Mining and Business Intelligence Analysis of E-Commerce Retail Data

Course Code	    DSA2040

Submission Date	14 December 2025

Student Name	MITCHELLE MORAA

Data Source	"Online Retail" (Transactional E-commerce Dataset)
Primary Libraries	pandas, numpy, sklearn, plotly.express, matplotlib, dash

**Introduction and Project Goals**

This project provides a detailed, multi-faceted analysis of a transactional e-commerce dataset ("Online Retail") spanning a 12-month period. The primary objective is to demonstrate proficiency in core data mining techniques and leverage these insights to construct an interactive Business Intelligence (BI) dashboard.

The analysis is structured into three main parts:

    Data Warehouse (BI) Analysis: Establishing core KPIs, sales trends, and geographic performance.

    Unsupervised Learning: Identifying customer segments (RFM/Clustering) and product co-purchase patterns (Association Rules).

    Supervised Learning: Visualizing feature separability for classification model justification.

**Methodology and Data Preparation**

3.1. DATA CLEANING AND ENGINEERING

The initial data preparation steps were crucial for ensuring the reliability of subsequent analysis:

    Missing Value Handling: Dropped rows where key identifiers (Description, Quantity, UnitPrice) were null.

    Transaction Filtering: Removed canceled transactions (InvoiceNo starting with 'C') and invalid entries (Quantity <= 0, UnitPrice <= 0).

    Sales Calculation: Engineered the core TotalSales column:
    TotalSales=Quantity×UnitPrice

3.2. RFM FEATURE CREATION

For customer segmentation, the Recency, Frequency, and Monetary (RFM) model was employed, summarizing customer behaviour into three key metrics:

    Recency (R): Days since the last purchase.

    Frequency (F): Total number of transactions.

    Monetary (M): Total money spent by the customer.

3.3. ETL CONFIGURATION AND STAR SCHEMA DESIGN

The initial phase built a robust ETL process, populating a relational data warehouse (retail_dw.db) using a Star Schema with one central Fact Table and three Dimension Tables (CustomerDim, ProductDim, TimeDim)
![alt text](<../Visualizations/Star Schema/Star Schema.jpg>)

**Part I: Data Warehouse & Business Intelligence (BI) Analysis**

4.1. QUARTERLY SALES TREND

To identify seasonality and growth patterns , the data was aggregated by calendar quarter.

Visualization	  

**Quarterly Sales Line Plot**

![alt text](<../Visualizations/Outputs/Total Revenue by Quartely Sales Trend.jpg>)

Insight

Clear evidence of a strong seasonal trend, with a sharp peak in Q4 (likely due to holiday shopping). The line plot is crucial for comparing year-over-year growth and forecasting.

Python

fig = px.line(
    df_quarterly,
    x='QuarterlyPeriod',
    y='TotalSales',
    title='Quarterly Sales Trend Over Time (Online Retail)',
    markers=True,
    labels={'TotalSales': 'Total Revenue (£)', 'QuarterlyPeriod': 'Reporting Period'}
)
fig.update_layout(
    xaxis_tickangle=-45,
    font=dict(size=12),
    xaxis_title='Time Period'
)
fig.show()
fig = px.line(df_quarterly, x='QuarterlyPeriod', y='TotalSales', title='Quarterly Revenue Trend')
fig.show()

4.2. GEOGRAPHIC AND PRODUCT PERFORMANCE

Geographic analysis is vital for strategic market focus, while product analysis identifies top revenue drivers.

Visualization	

**Top 10 Countries Geographic visualization**

![alt text](<../Visualizations/Outputs/Geographic Visualization.jpg>)	

Insight

Identifies the most significant revenue contributions by country. Typically, the United Kingdom dominates, highlighting the need for market diversification strategies.
Product Sales Treemap	Visually represents the product mix where the size of each block is proportional to its revenue contribution. Reveals the "Regency Cakestand 3 Tier" (or similar product) as the highest earner.

4.3 VISUALIZATION

A. **Feature Correlation Heatmap**

![alt text](<../Visualizations/Outputs/Correlation Heatmap.png>)

Feature Selection Justification: Visualizes the Pearson correlation matrix between the scaled RFM features. Essential for identifying collinearity, which can bias distance-based algorithms like K-Means. 

B. **Kernel Density Estimate (KDE) Plots**

![alt text](<../Visualizations/Outputs/KDE Plots.png>)

Distribution Analysis: Displays the probability distribution of individual RFM features and justifying the necessary log-transformation and standardization prior to K-Means application.

C. **Violin Plots**

![alt text](<../Visualizations/Outputs/Violin plots.png>)

Segment Validation: Shows the full distribution, including median and quartiles, of a feature  across the different K-Means clusters. 

**Part II: Unsupervised Learning**

5.1. K-MEANS CLUSTERING FOR CUSTOMER SEGMENTATION

K-Means was applied to the scaled RFM features to segment customers into actionable groups.

5.1.1. OPTIMAL K SELECTION (ELBOW METHOD)

The Elbow Method was used to determine the optimal number of clusters (K) by plotting the Distortion (Inertia) against K. The "elbow" (point of diminishing returns) typically suggested K=3 or K=4.

![alt text](<../Visualizations/Outputs/Elbow plot.png>)

5.1.2. CLUSTER VISUALIZATION

The results of the clustering were visualized on key features (e.g., Petal Length vs. Petal Width for Iris, or Monetary vs. Frequency for RFM).

Visualization	
.
![alt text](<../Visualizations/Outputs/k-means clustering k=2.jpg>)
![alt text](<../Visualizations/Outputs/K-Means Clustering.jpg>)
![alt text](<../Visualizations/Outputs/K-Means clustering k=4.jpg>)

Insights 

Cluster Scatter Plot	Shows the final customer groups defined by the cluster IDs. For RFM, this helps visually separate High-Value Customers (high M, high F) from Churn Risk (low R, low F, low M)


5.2. ASSOCIATION RULE MINING (Apriori Algorithm)

Association Rule Mining was performed to discover strong product co-purchase patterns (e.g., {Item A}→{Item B}).

Visualization	

Lift vs. Confidence Scatter Plot	

![alt text](<../Visualizations/Outputs/Lift vs Confidence.jpg>)

Insight

This is the most crucial output. Rules above the Lift = 1.0 dashed line indicate a positive, non-random relationship between items. High Lift + High Confidence rules are the most actionable for product recommendations and basket analysis.

Python

fig = px.scatter(
    rules_df, x='confidence', y='lift', size='support',
    hover_data={'antecedents_str': True, 'consequents_str': True})
fig.add_hline(y=1.0, line_dash="dash", line_color="red")
fig.show()

**Part III: Supervised Learning (Classification Justification)**

To support the comparison of classifiers like Decision Trees (DT) and K-Nearest Neighbors (KNN) (e.g., using the Iris dataset), a visualization of feature separability is essential.

Visualization	

Feature Scatter Plot (True Labels)

![alt text](<../Visualizations/Outputs/Iris Data.jpg>)

Insight

Plots two discriminative features (e.g., Petal Length vs. Petal Width) colored by the True Class Label. This plot visually justifies the high accuracy of simple classifiers like DT, showing where clear, linear splits are possible.

**Final Deliverable: Comprehensive Analysis Dashboard**

The final deliverable is an interactive, web-based dashboard built using Plotly Dash that consolidates the key insights from all analytical components into one view. The dashboard layout is designed to group similar analyses logically (Sales, Customer/ML).

7.1. DASHBOARD COMPONENTS AND LAYOUT

Row	Left Component (50% Width)	Right Component (50% Width)

1	A. Product Mix Revenue Share (Treemap)	

![alt text](<../Visualizations/Outputs/Total Sales by Product.jpg>)

B. Quarterly Revenue Trend (Line Plot)

2	C. Top 10 Countries by Total Revenue

D. Customer Monetary Value Distribution (Histogram/RFM)

3	E. Association Rules: Lift vs. Confidence (Scatter Plot)	N/A (Full Width)

7.2. DASHBOARD EXECUTION CODE

The following Python code launches the interactive dashboard.
Python

import dash
from dash import dcc, html
import plotly.express as px
import pandas as pd
import numpy as np 

app = dash.Dash(__name__)

app.layout = html.Div(style={'backgroundColor': '#f0f2f5', 'padding': '20px'}, children=[
    
    html.H1(children='Online Retail Comprehensive Analysis Dashboard', 
            style={'textAlign': 'center', 'color': '#1f2937'}),

    # Row 1: Core Sales Performance
    html.Div(style={'display': 'flex', 'flexDirection': 'row', 'marginBottom': '20px'}, children=[
        html.Div(style={'width': '50%', 'padding': '15px', 'backgroundColor': 'white', 'boxShadow': '0 4px 8px 0 rgba(0,0,0,0.1)'}, children=[
            dcc.Graph(figure=create_treemap())
        ]),
        html.Div(style={'width': '50%', 'padding': '15px', 'backgroundColor': 'white', 'boxShadow': '0 4px 8px 0 rgba(0,0,0,0.1)', 'marginLeft': '20px'}, children=[
            dcc.Graph(figure=create_trend_plot())
        ]),
    ]),
    
    # Row 2: Customer and Geographic Insights
    html.Div(style={'display': 'flex', 'flexDirection': 'row', 'marginBottom': '20px'}, children=[
        html.Div(style={'width': '50%', 'padding': '15px', 'backgroundColor': 'white', 'boxShadow': '0 4px 8px 0 rgba(0,0,0,0.1)'}, children=[
            dcc.Graph(figure=create_country_bar_chart())
        ]),
        html.Div(style={'width': '50%', 'padding': '15px', 'backgroundColor': 'white', 'boxShadow': '0 4px 8px 0 rgba(0,0,0,0.1)', 'marginLeft': '20px'}, children=[
            dcc.Graph(figure=create_monetary_distribution())
        ]),
    ]),
    
    # Row 3: Machine Learning/Strategic Insight
    html.Div(style={'padding': '15px', 'backgroundColor': 'white', 'boxShadow': '0 4px 8px 0 rgba(0,0,0,0.1)'}, children=[
        dcc.Graph(figure=create_rules_scatter())
    ]),
])

if __name__ == '__main__':
    print("Dashboard starting... Visit http://127.0.0.1:8050/ in your browser.")
    # Corrected method to run the Dash application
    app.run(debug=True)

**Conclusion**

The comprehensive analysis successfully utilized various data mining techniques to derive actionable business insights. The BI component established core performance metrics across time and geography. Unsupervised methods yielded distinct customer segments via K-Means and high-impact product recommendation rules via Apriori. All key findings were integrated into a professional, interactive Plotly Dash dashboard, fulfilling the core requirements of the project.