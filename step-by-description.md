# Data Analysis Project

**Objective:** This project aims to consolidate the knowledge acquired during a Pandas course, where I learned fundamental data cleaning and preprocessing techniques. The goal was to work with a new dataset without any instructions or step-by-step guidance, allowing me to practice handling real-world data independently.

I chose a Brazilian e-commerce dataset containing information about orders, customers, sellers, reviews, prices, and other related data. The objective was to clean and prepare the datasets, identify data quality issues, and solve them appropriately. In addition, I performed exploratory data analysis to uncover relevant insights and better understand the data.

This repository contains the complete project, including a Jupyter Notebook with the full implementation. The notebook presents the complete workflow, while this file (step-by-description) has detailed explanations of each step, assumptions, and findings, and the README file provides a concise summary of the project and its main results.

## 1. Initial Treatment

After exploring the datasets, examining their columns, shapes, and how they were likely related to each other, I started by applying some basic data cleaning procedures.

### 1.1. NULL Values

During the initial exploration, I found three datasets containing missing values, and I applied a different strategy to each one:

- **df_order_reviews:** The missing values were only in the review title and review comment columns. For this project, this was not an issue because the most important information, the review score (star rating), was complete for all records. Therefore, I replaced the missing text fields with "no_comment" to make the missing information explicit.

- **df_products:** This dataset presented two different types of missing values:
    - First case: Missing descriptive information (such as text attributes). Since the information was unavailable, I filled these values with "no_information" to explicitly indicate that the data was missing.
    - Seconde case: Missing numerical attributes (such as product dimensions and weight). In this case, I replaced the missing values with the mean value of the corresponding product category. Since there were only a few missing records, using the category mean was a reasonable imputation strategy.

- **df_orders:** The missing values were found in date-related columns. However, these missing values were expected because those columns depend on the order status. For example, the delivery date is only available after an order has been delivered. Therefore, orders that were canceled or had not yet been completed naturally contained missing values in these fields. For this reason, I decided to keep these missing values unchanged.

### 1.2. Duplicated Values

After to check the missing values, I started to check the duplicated values. So I try to think what was the columns that must be unqiue in each dataset and how to see this. So I compare the ids columns and the len of dataframe and for each case I decide to apply a strategy:

- **df_order_payments:** 

- **df_order_reviews:** 

- **df_geolocation:** in this dataset, we had the granularity of the zip code and the information of latitude and longitude. It is not wrong, but for the purpose that I was thinking, I had to keep a city/state granularity. So I had to apply some transformations, described in the next session.

- **df_order_items:** 



For the others dataset, the duplicated values of ids make sense with the shape of dataframes.

### X. The Datasets

##### **1. df_customer**

| column                               | description                                                             |
|--------------------------------------|-------------------------------------------------------------------------|
| `customer_id`                        | key to the order dataset; each order has a unique customer_id           |
| `customer_unique_id`                 | unique identifier of the customer                                       |
| `customer_zip_code_prefix`           | first five digits of the ZIP code (without leading zeros)               |
| `customer_city`                      | customer city name                                                      |
| `customer_state`                     | customer state (UF/FS)                                                  |

##### **2. df_geolocation**

| column                               | description                                                             |
|--------------------------------------|-------------------------------------------------------------------------|
| `geolocation_zip_code_prefix`        | first five digits of the ZIP code (without leading zeros)               |
| `geolocation_lat`                    | latitude coordinates                                                    |
| `geolocation_lng`                    | longitude coordinates                                                   |
| `geolocation_city`                   | city name                                                               |
| `geolocation_state`                  | federative state                                                        |

##### **3. df_order_items**

| column                               | description                                                             |
|--------------------------------------|-------------------------------------------------------------------------|
| `order_id`                           | order unique id                                                         |
| `order_item_id`                      | sequential number identyfing number of items included in the same order |
| `product_id`                         | product unique idetifier                                                |
| `seller_id`                          | seller unique identifier                                                |
| `shipping_limit_date`                | seller shipping limit date to handling the order to logistic partner    |
| `price`                              | item price (for each product)                                           |
| `freight_value`                      | freight value (for each product)                                        |

##### **4. df_order_payments**

| column                               | description                                                             |
|--------------------------------------|-------------------------------------------------------------------------|
| `order_id`                           | order unique id                                                         |
| `payment_sequential`                 | if the customer pay order with more than one method  it'll show here    |
| `payment_type`                       | method of payment chosen by the customer                                |
| `payment_installments`               | number of installments chosen by the customer                           |
| `payment_value`                      | transaction value                                                       |

##### **5. df_order_reviews**

| column                               | description                                                             |
|--------------------------------------|-------------------------------------------------------------------------|
| `review_id`                          | review unique id                                                        |
| `order_id`                           | order unique id                                                         |
| `review_score`                       | note of the satisfaction survey                                         |
| `review_comment_title`               | comment title from the review (Portuguese)                              |
| `review_comment_message`             | comment message from the review (Portuguese)                            |
| `review_creation_date`               | date when satisfaction survey was sent to the customer                  |
| `review_answer_timestamp`            | date when satisfation suvery was answered                               |

##### **6. df_orders**

| column                               | description                                                             |
|--------------------------------------|-------------------------------------------------------------------------|
| `order_id`                           | order unique id                                                         |
| `customer_id`                        | customer unique id                                                      |
| `order_status`                       | order status                                                            |
| `order_purchase_timestamp`           | purchase timestamp                                                      |
| `order_approved_at`                  | payment approval timestamp                                              |
| `order_delivered_carrier_date`       | order posting timestamp (handled to the logist partner)                 |
| `order_delivered_customer_date`      | order delivered to the customer                                         |
| `order_estimated_delivery_date`      | the estimated delivery date showed to the customer                      |


##### **7. df_products**

| column                               | description                                                             |
|--------------------------------------|-------------------------------------------------------------------------|
| `product_id`                         | product unique identifier                                               |
| `product_category_name`              | root category of product (Portuguese)                                   |
| `product_name_lenght`                | number of characters from the product name                              |
| `product_description_lenght`         | number of characters from the description                               |
| `product_photos_qty`                 | number of product published photos                                      |
| `product_weight_g`                   | product weight measured in grams                                        |
| `product_length_cm`                  | product lenght measured in centimeters                                  |
| `product_height_cm`                  | product height measured in centimeters                                  |
| `product_width_cm`                   | product width measured in centimeters                                   |

##### **8. df_sellers**

| column                               | description                                                             |
|--------------------------------------|-------------------------------------------------------------------------|
| `seller_id`                          | seller unique identifier                                                |
| `seller_zip_code_prefix`             | first five digits of the seller ZIP code (without leading zeros)        |
| `seller_city`                        | seller city                                                             |
| `seller_state`                       | seller state (UF/FS)                                                    |

##### **9. df_product_translation**

| column                               | description                                                             |
|--------------------------------------|-------------------------------------------------------------------------|
| `product_category_name`              | category name in Portuguese                                             |
| `product_category_name_english`      | category name in English                                                |