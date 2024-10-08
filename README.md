Dataset has been drawn from "https://www.kaggle.com/c/instacart-market-basket-analysis/data".

### Objectives:
- Analyze the anonymized [data](https://www.kaggle.com/c/instacart-market-basket-analysis/data) of 3 million grocery orders from more than 200,000 Instacart users open sourced by Instacart 
- Find out hidden association between products for better cross-selling and upselling
- Perform customer segmentation for targeted marketing and anticipate customer behavior
- Build a Machine Learning model to predict which previously purchased product will be in user’s next order

### Project Organization
```
.
├── Plots/                                      : Contains all plots 
├── Data Description and Analysis.ipynb         : Initial analysis to understand data
├── Exploratory Data Analysis.ipynb             : EDA to analyze customer purchase pattern
├── Customers Segmentation.ipynb                : Customer Segmentation based on product aisles
├── Market Basket Analysis.ipynb                : Market Basket Analysis to find products association
├── Feature Extraction.ipynb                    : Feature engineering and extraction for a ML model
├── Data Preparation.ipynb                      : Data preparation for modeling
├── ANN Model.ipynb                             : Neural Network model for product reorder prediction
├── XGBoost Model.ipynb                         : XGBoost model for product reorder prediction
├── LICENSE                                     : License
└── README.md                                   : Project Report 
```
<br />

## Data Description

- **aisles:** This file contains different aisles and there are total 134 unique aisles.

- **departments:** This file contains different departments and there are total 21 unique departments.

- **orders:** This file contains all the orders made by different users. From below analysis, we can conclude following:
    - There are total 3421083 orders made by total 206209 users.
    - There are three sets of orders: Prior, Train and Test. The distributions of orders in Train and Test sets are similar whereas the distribution of orders in Prior set is different.
    - The total orders per customer ranges from 0 to 100.
    - Based on the plot of 'Orders VS Day of Week' we can map 0 and 1 as Saturday and Sunday respectively based on the assumption that most of the people buy groceries on weekends.
    - Majority of the orders are made during the day time.  
    - Customers order once in a week which is supported by peaks at 7, 14, 21 and 30 in 'Orders VS Days since prior order' graph.
    - Based on the heatmap between 'Day of Week' and 'Hour of Day,' we can say that Saturday afternoons and Sunday mornings are prime time for orders.
    
<p align="center">
  <img width="300" height="200" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/dow.png">
</p>

<p align="center">
  <img width="600" height="300" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/orders.png">
</p>  

<p align="center">
  <img width="600" height="300" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/heatmap.png">
</p>

- **products:** This file contains the list of total 49688 products and their aisle as well as department. The number of products in different aisles and different departments are different.

- **order_products_prior:** This file gives information about which products were ordered and in which order they were added in the cart. It also tells us that if the product was reordered or not.
    
    - In this file there is an information of total 3214874 orders through which total 49677 products were ordered.
    - From the 'Count VS Items in cart' plot, we can say that most of the people buy 1-15 items in an order and there were a maximum of 145 items in an order.
    - The percentage of reorder items in this set is 58.97%.
    
<p align="center">
  <img width="600" height="300" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/prior.png">
</p>
    
    
- **order_products_train:** This file gives information about which products were ordered and in which order they were added in the cart. It also tells us that if the product was reordered or not.
    - In this file there is an information of total 131209 orders through which total 39123 products were ordered.
    - From the 'Count VS Items in cart' plot, we can say that most of the people buy 1-15 items in an order and there were a maximum of 145 items in an order.
    - The percentage of reorder items in this set is 59.86%.

<p align="center">
  <img width="600" height="300" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/train.png">
</p>
    
## Exploratory Data Analysis
For the analysis I combined all of the separate data files into one single dataframe and to fit the dataframe in my memory I reduced its size to 50% (4.1 GB to 2.0 GB) by type conversion and without loosing any information.

- This plot shows most popular aisles based on total products bought.

<p align="center">
  <img width="600" height="300" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/popular-aisles.png">
</p>

- As we can see in below plot that the reorder percentage of day-to-day food items is high and for other products such as vitamins, first-aids, beauty products, etc. reorder percentage is low. This is true as we buy only groceries regularly and do not buy those items in every order.

<p align="center">
<img width="400" height="220" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/aisle-high-reorder.png"> 
<img width="400" height="220" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/aisle-low-reorder.png"> 
<p/>

- The below plot shows popular departments. The store layout should be in a way that popular departments are very near to each other.

<p align="center">
  <img width="600" height="300" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/popular-departments.png">
</p>

- The below plot shows most popular products. As we can see there are many organic products in the most popular products.

<p align="center">
  <img width="600" height="300" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/Most-popular-products.png">
</p>

- We can see that there are less number of organic products but their Mean reorder percentage is high. This tells us that we should have more organic products in the store.

<p align="center">
    <img width="400" height="250" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/Total-organic-inorganic-products.png"/> 
    <img width="400" height="250" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/Reorder-organic-inorganic-products.png"/>
</p>


- We can plot add-to-cart-order and mean reorder percentage. As we can see the lower the add-to-cart-order higher is the reorder percentage. This makes sense as we mostly buy things first that are required on day-to-day basis.

<p align="center">
  <img width="600" height="300" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/Add-to-cart-VS-reorder.png">
</p>

- In the below plot of reorder percentage and number of product purchase, we see a ceiling effect. Many people try different product once and they do not reorder again. Also, there are users who buy certain products regularly. 

<p align="center">
  <img width="600" height="300" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/reorder-total-orders.png">
</p>


<p align="center">
  <img width="500" height="400" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/reorder-df.png">
</p>


<p align="center">
  <img width="600" height="300" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/cumsum_products.png">
</p>

## Customer Segmentation


<p align="center">
  <img width="600" height="300" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/elbow.png">
</p>

The clustering can be visualized along first two principal components as below.

<p align="center">
  <img width="600" height="400" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/cluster.png">
</p>

The clustering results into 5 neat clusters and after checking most frequent products in them, we can conclude following:
- Cluster 1 results into 5428 consumers having a very strong preference for water seltzer sparkling water aisle.
- Cluster 2 results into 55784 consumers who mostly order fresh vegetables followed by fruits.
- Cluster 3 results into 7948 consumers who buy packaged produce and fresh fruits mostly.
- Cluster 4 results into 37949 consumers who have a very strong preference for fruits followed by fresh vegetables.
- Cluster 5 results into 99100 consumers who orders products from many aisles. Their mean orders are low compared to other clusters which tells us that either they are not frequent users of Instacart or they are new users and do not have many orders yet. 

## Markest Basket Analysis

Market Basket Analysis is a modelling technique based upon the theory that if you buy a certain group of items, you are more or less likely to buy another group of items. Market basket analysis may provide the retailer with information to understand the purchase behavior of a buyer. This information can then be used for purposes of cross-selling and up-selling, in addition to influencing sales promotions, loyalty programs, store design, and discount plans.

Market basket analysis scrutinizes the products customers tend to buy together, and uses the information to decide which products should be cross-sold or promoted together. The term arises from the shopping carts supermarket shoppers fill up during a shopping trip.

Association Rule Mining is used when we want to find an association between different objects in a set, find frequent patterns in a transaction database, relational databases or any other information repository.

The most common approach to find these patterns is Market Basket Analysis, which is a key technique used by large retailers like Amazon, Flipkart, etc to analyze customer buying habits by finding associations between the different items that customers place in their “shopping baskets”. The discovery of these associations can help retailers develop marketing strategies by gaining insight into which items are frequently purchased together by customers. The strategies may include:

- Changing the store layout according to trends
- Customers behavior analysis
- Catalog Design
- Cross marketing on online stores
- Customized emails with add-on sales, etc.



## ML Model to Predict Product Reorders

We can utilize this anonymized transactional data of customer orders over time to predict which previously purchased products will be in a user’s next order. This would help recommend the products to a user. 

To build a model, I need to extract features from previous order to understand user's purchase pattern and how popular the particular product is. I extract following features from the user's transactional data.

**Product Level Features:** To understand the product's popularity among users
```
(1) Product's average add-to-cart-order
(2) Total times the product was ordered
(3) Total times the product was reordered
(4) Reorder percentage of a product
(5) Total unique users of a product
(6) Is the product Organic?
(7) Percentage of users that buy the product second time
```

**Aisle and Department Level Features:** To capture if a department and aisle are related to day-to-day products (vegetables, fruits, soda, water, etc.) or once-in-a-while products (medicines, personal-care, etc.) 
```
(8) Reorder percentage, Total orders and reorders of a product aisle
(9) Mean and std of aisle add-to-cart-order
(10) Aisle unique users
(10) Reorder percentage, Total orders and reorders of a product department
(11) Mean and std of department add-to-cart-order
(12) Department unique users
(13) Binary encoding of aisle feature (Because one-hot encoding results in many features and make datarame sparse)
(14) Binary encoding of department feature (Because one-hot encoding results in many features and make datarame sparse)
```

### ML Models

Using the extracted features, I prepared a dataframe which shows all the products user has bought previously, user level features, product level features, asile and department level features, user-product level features and the information of current order such as order's day-of-week, hour-of-day, etc. The Traget would be 'reordered' which shows how many of the previously purchased items, user ordered this time. 

Since the dataframe is huge, I reduced the memory consumption of it by downcasting to fit the data int my memory. I preferred MinMaxScaler over StandardScaler as the latter requires 16 GB of RAM for its operation. I followed standard process for model building and I relied on XGBoost as it handles large data, can be parallelized and gives feature importance. I also built Neural Network to see what would be the best performance from this model disregarding some inherent randomness from both of these models.  To balance the data, I have used cost-sensitive learning by assigning class weightage (~{0:1, 1:10}). I have not used random-upsampling/SMOTE as it would increase the data size and I do not have much memory. Also, since random-down-sampling discards information which might be important and would result in bias. 

Since, we can hack the F1 score by changing the threshold, I relied on AUC Score for model evaluation. The performance of both of these models is shown below using Confusion Matrix, ROC curve and classification report. The feature important plot from XGBoost model is also shown to understand important features which help predict product's reorder. The performance of both models is almost similar and XGBoost slightly performs better in terms of ROC-AUC.

**Neural Network Model Architecture and Performance:**

<p align="center">
  <img width="400" height="200" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/NN%20Architecture.png">
</p>

<p align="center">
  <img width="400" height="200" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/NN-Report.png">
</p>

<p align="center">
  <img width="600" height="300" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/NN-Performance.png">
</p>


**XGBoost Model's Performance and Feature Importance:**

<p align="center">
  <img width="400" height="200" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/XGBoost-Report.png">
</p>

<p align="center">
  <img width="600" height="300" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/XGBoost%20Performance.png">
</p>

<p align="center">
  <img width="500" height="750" src="https://github.com/archd3sai/Instacart-Market-Basket-Analysis/blob/master/Plots/XGBoost%20Feature%20Importance%20Plot.png">
</p>

