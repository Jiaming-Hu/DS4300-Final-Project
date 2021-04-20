# DS4300-Final-Project

1. Introduction

Product recommendation engines are now a common feature on emerce sites. By using algorithms, they are used to surface products for customers based on various factors. While we are looking for products, we find a section containing items similar to  what we are viewing at the moment. Behind that, they’re the recommend algorithms. 

They benefit retailers by showing visitors the products that they ‘re most likely to buy, based on the data fed into the engine. For the shopper, the improved relevance means a better shopping experience. In our project, we implemented two types of recommendation algorithms (Content-Based Filtering and Collaborative Filtering) to create a non-trivial recommendation engine for Amazon products based on customer’s reviews.

2. Data

Our datasets are taken from kaggles.com for free, it contains over 63595 consumer reviews for Amazon products like Kindle, Fire TV Stick... Basic product information, product ratings, review text are also included in the dataset. Since our goal is to make recommendations based on reviews, after data cleaning, our data includes product id, product name, product manufacturer, product categories, user id, review text, product rating and user’s age.

	Nodes:
(u: User {username, age})
(c: Category {category})
(p: Product {id, name, brand, manufacturer})

	Relationship:
(p: Product)--[: In]-->(c: Category)
(u: User)--[:Reviews {id, rating, text}]-->(p: Product)

To satisfy our needs, we have three types of nodes and two types of relationships in our database. For every customer node there is a review relation link to product node and for product node there is a in category relation link to category. There are overall 346 categories, 92 products and 34000 users. 



3. Technical Features 

3.1 Import data
Before importing data, three constraints are constructed which prevent repeating category nodes, user nodes and product nodes. Instead of importing data from csv files directly to neo4j, we connect python with Neo4j using Nep4j Python Driver and write a few functions that first load data into data frames and then import data from dataframe to neo4j. 


3.2 Content Based Filtering 
This algorithm allows us to search and suggest contents that are very similar to the ones the user had reviewed, or bought. To do this, we use an index called jacquard index which is a measurement of the similarity between two sets. If two sets are identical then the Jaccard index would be 1.0; if there are no common elements between two sets, then the Jaccard index would be 0.0. Jaccard index is a built- in index in Neo4j, but in order to understand how it works, we decided to calculate it ourselves. From our research, the Jaccard index is calculated by the size of the intersection of two sets divided by the size of the union of two sets. By employing Jaccard index, our function would take in a username and then output a recommended product. 


3.3 User based collaborative filtering
This algorithm allows us to make suggestions on products based on users who share a similar interest as the user we want to make suggestions for. To be more specific, at first, we use the powerful cql to match the users who bought the same product as the selected user and get other products they bought. Rated these products by how many products the selected user and user bought in common and how many new products the user has and selected user doesn’t have, then we could get the top 25 products as the result. The specific cql is at below. User kacy is an example.


3.4 Collaborative filtering by user’s age
This algorithm allows us to make suggestions on products based on users’ age. In the data cleaning process, we randomly assign ages to our users. Here, with the given username, we first get his/her name from the database. Next, calculate the lower bound and upper bound of the age range with the given difference. Finally, we get the products that are highly reviewed by users that are aged between the lower bound and the upper bound. 





4. Contribution:

Kunhan Wu: Write user based collaborative filtering algorithm, create database, fix the relationship and data loading error.
Anni Yuan: Content based filtering, data importing functions and user age generation.
Jiaming Hu: Data Cleaning, collaborative filtering by age algorithm, refine the design of the data import methods
