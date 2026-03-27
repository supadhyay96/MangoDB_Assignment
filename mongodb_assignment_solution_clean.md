# MongoDB Assignment Solution

------------------------------------------------------------------------

# Theoretical Questions

## 1. Key Differences Between SQL and NoSQL

  SQL                   NoSQL
  --------------------- -------------------------
  Relational database   Non-relational database
  Fixed schema          Flexible schema
  Tables                Collections
  Vertical scaling      Horizontal scaling
  Structured data       Semi/unstructured data

## 2. What makes MongoDB a good choice for modern applications

-   Flexible schema
-   High scalability
-   JSON-like documents
-   High performance
-   Easy integration with modern applications

## 3. Concept of Collections in MongoDB

Collections are groups of documents stored together, similar to tables
in relational databases.

## 4. High availability using replication

MongoDB uses replica sets: - Primary node handles write operations -
Secondary nodes replicate data - Automatic failover ensures availability

## 5. Benefits of MongoDB Atlas

-   Managed cloud database
-   Automated backups
-   Monitoring tools
-   Global deployment
-   Built‑in security

## 6. Role of Indexes

Indexes improve query performance by reducing the number of scanned
documents.

## 7. Aggregation Pipeline Stages

-   `$match`
-   `$group`
-   `$sort`
-   `$project`
-   `$lookup`

## 8. Sharding vs Replication

  Sharding           Replication
  ------------------ -----------------------
  Distributes data   Copies data
  Used for scaling   Used for availability

## 9. PyMongo

PyMongo is the Python driver used to connect and work with MongoDB
databases.

## 10. ACID Properties

-   Atomicity
-   Consistency
-   Isolation
-   Durability

## 11. explain() Function

Used to analyze query execution plans and performance.

## 12. Schema Validation

MongoDB allows JSON schema validation to enforce document structure.

## 13. Primary vs Secondary Node

Primary → accepts writes\
Secondary → replicates data

## 14. Security Mechanisms

-   Authentication
-   Authorization
-   Encryption
-   Role-based access control

## 15. Embedded Documents

Nested documents inside another document, useful when related data is
accessed together.

## 16. `$lookup`

Used to perform join operations between collections.

## 17. Common Use Cases

-   Real-time analytics
-   E-commerce platforms
-   IoT applications
-   Content management systems

## 18. Advantages of Horizontal Scaling

-   Handles large datasets
-   Improves performance
-   Distributed architecture

## 19. MongoDB vs SQL Transactions

MongoDB transactions support multi-document operations optimized for
distributed systems.

## 20. Capped vs Regular Collections

  Capped           Regular
  ---------------- -----------------
  Fixed size       Dynamic size
  Auto overwrite   Manual deletion

## 21. `$match`

Filters documents similar to SQL WHERE clause.

## 22. Securing MongoDB Access

-   Enable authentication
-   Use TLS encryption
-   IP whitelisting
-   Strong passwords

## 23. WiredTiger Storage Engine

Default MongoDB storage engine providing compression, concurrency
control, and improved performance.

------------------------------------------------------------------------

# Practical Questions (Python + MongoDB)

## Install Requirements

``` bash
pip install pymongo pandas
```

## Python Script

``` python
from pymongo import MongoClient
import pandas as pd

# Connect to MongoDB
client = MongoClient("mongodb://localhost:27017/")
db = client["superstoreDB"]
collection = db["Orders"]

# Load CSV into MongoDB
df = pd.read_csv("data/superstore.csv")
data = df.to_dict(orient="records")
collection.insert_many(data)

# Retrieve all documents
for doc in collection.find().limit(5):
    print(doc)

# Count total documents
print("Total Documents:", collection.count_documents({}))

# Fetch orders from West region
west_orders = collection.find({"Region": "West"})
for order in west_orders.limit(5):
    print(order)

# Orders where Sales > 500
sales_gt_500 = collection.find({"Sales": {"$gt": 500}})
for order in sales_gt_500.limit(5):
    print(order)

# Top 3 highest Profit orders
top_profit = collection.find().sort("Profit", -1).limit(3)
for order in top_profit:
    print(order)

# Update Ship Mode
collection.update_many(
    {"Ship Mode": "First Class"},
    {"$set": {"Ship Mode": "Premium Class"}}
)

# Delete orders with Sales < 50
collection.delete_many({"Sales": {"$lt": 50}})

# Aggregation: total sales per region
pipeline = [
    {"$group": {"_id": "$Region", "TotalSales": {"$sum": "$Sales"}}}
]

for r in collection.aggregate(pipeline):
    print(r)

# Distinct Ship Modes
print(collection.distinct("Ship Mode"))

# Count orders per category
category_pipeline = [
    {"$group": {"_id": "$Category", "OrderCount": {"$sum": 1}}}
]

for r in collection.aggregate(category_pipeline):
    print(r)
```
