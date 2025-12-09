---
title: "Blog 3"
date: "2025-11-30"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

## [AWS Database Blog](https://aws.amazon.com/blogs/database/)

# Evolve Your Amazon DynamoDB Table Data Model

By Imhoertha Ojior | July 10, 2025 | In [Advanced (300)](https://aws.amazon.com/blogs/database/category/learning-levels/advanced-300/), [Amazon DynamoDB](https://aws.amazon.com/blogs/database/category/database/amazon-dynamodb/), [Technical How-to](https://aws.amazon.com/blogs/database/category/post-types/technical-how-to/) | [Permalink](https://aws.amazon.com/blogs/database/evolve-your-amazon-dynamodb-tables-data-model/) | [Comments](https://aws.amazon.com/blogs/database/evolve-your-amazon-dynamodb-tables-data-model/#Comments) | [Share](https://aws.amazon.com/blogs/database/evolve-your-amazon-dynamodb-tables-data-model/#)

Most software applications evolve over time, often requiring changes to database schema. [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) provides **schemaless design** – you only need to define the attributes of the primary key when you create a table. This flexibility allows you to store both structured and semi-structured data, making it possible to update your data model without requiring service downtime.

In this post, we will show you how to **evolve your DynamoDB table data model** to meet changing application requirements while maintaining **zero downtime** in your production system. We will explore two key techniques with examples that you can apply to your own application:

* Adding new attributes
* Creating new entities

---

### **Adding New Attributes to Items in Your Table**

As your application features evolve, you may need to add new attributes to store data for these new features. Depending on your feature requirements, you can start creating new items without needing to update existing items first.

When your application requires a new attribute in your table's data model that doesn't affect the application's access patterns, you can add that attribute to new items being written to the table.

If you have an access pattern that depends on the new attribute, you can create a [global secondary index](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GSI.html) (GSI) using this new attribute in its primary key. Items will be added to the GSI as new items are created with the new attribute or when existing items are updated to include this attribute.

Consider a DynamoDB table containing orders from a grocery website. Each order is identified by a partition key (CustomerId) and a sort key (OrderId). The following figure illustrates the data model of this DynamoDB table.

![][image1]

We add a new attribute called DeliveryDate and enable a new access pattern by using a GSI called DeliveryDate-Index. This allows the application to search for orders based on the order's delivery date. The following figure illustrates the data model for the updated DynamoDB table.

![][image2]

The following figure shows the data model for the DeliveryDate-Index GSI.

![][image3]

With this update, we can retrieve orders delivered on a specific date from the DynamoDB table. The following Python code example illustrates how to retrieve orders from the DynamoDB table by delivery date:

```python
try:
    # Query grocery orders by a delivery date
    response = client.query(
        TableName='Orders',
        IndexName='DeliveryDate-index',
        KeyConditionExpression='DeliveryDate = :delivery_date',
        ExpressionAttributeValues={
            ':delivery_date': { 'S': '2025-03-21' }
        }
    )
    return response
except Exception as e:
    print(f"An error occurred: {str(e)}")
    return []
```

For existing items in the table that don't yet have the new attribute, you can update your application to accept the case where some items don't have this attribute when retrieving data, and then gradually set the attribute for those items. This approach can increase the operational complexity of the table because you will have to manage multiple versions of the same entity for an extended period. Alternatively, you can perform a bulk update of items in the DynamoDB table to set the new attribute. This approach is faster and operationally less complex, but will incur costs due to having to update all items that need the new attribute.

The tradeoff here is between the time needed to gradually update existing items versus the cost of performing a bulk update. The best approach depends on your specific use case, table size, and application requirements. For new attributes that are essential to most of your application's functionality, updating existing items before creating new items is a reasonable choice. For optional new attributes or new attributes in a very large table, a phased approach, starting with new items, may be more practical.

If your application requires all existing items to have the new attribute before creating new items, you can bulk update items in your DynamoDB table using [AWS Step Functions](https://aws.amazon.com/step-functions/) (see [Bulk update Amazon DynamoDB tables with AWS Step Functions](https://aws.amazon.com/blogs/database/bulk-update-amazon-dynamodb-tables-with-aws-step-functions/)) or [AWS Glue](https://aws.amazon.com/glue/).

See [Cost-effective bulk processing with Amazon DynamoDB](https://aws.amazon.com/blogs/database/cost-effective-bulk-processing-with-amazon-dynamodb/) to learn how to perform cost-effective bulk updates for your DynamoDB table.

---

### **Adding a New Entity to Your Table**

Because DynamoDB has a flexible schema, you can efficiently store and retrieve different entities in the same DynamoDB table without needing to perform JOINs.

When your application requires adding a new entity to your table's data model, the process is similar to adding a new attribute. If the properties of the new entity are not used by the application in an access pattern, you can add the new entity to the data model and then perform a bulk update of the DynamoDB table if needed.

If a nested attribute in a new or existing entity is used by the application in an access pattern, you can create a new entity in the table to support that access pattern.

Consider a DynamoDB table containing orders from a grocery website. Each order is identified by a partition key (CustomerId) and a [composite sort key](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/data-modeling-blocks.html#data-modeling-blocks-composite) (OrderDate#OrderId), and each order contains a list of grocery items purchased. The following figure illustrates the data model for this DynamoDB table.

![][image4]

As the website grows, we want to allow customers to shop from previous orders by marking items in previous orders as favorite items. The shopper can then retrieve the list of items that have previously been tagged as favorites when they place new orders.

To support these features on the website, we add a **nested Boolean Favourite attribute** to the **Items entity** in the Orders table. This allows shoppers to mark items in their grocery orders as favorite items.

We also add a **new entity** identified by a **partition key (CustomerId)** and a **composite sort key (FAVOURITE#ItemId)** to the Orders table. We use a **composite sort key with a prefix – FAVOURITE#** for this new entity so that we can retrieve all favorite items for a given CustomerId by using the begins_with range query operator.

Refer to the *DynamoDB Developer Guide* to learn more about [**best practices for using sort keys to organize data in DynamoDB**](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-sort-keys.html).

The following figure illustrates the updated data model for the DynamoDB table.

![][image5]

With this update, we can tag a grocery item in an order as a favorite item when writing new orders to the DynamoDB table. We can also retrieve the list of favorite items for a shopper. The following Python code example illustrates how to retrieve favorite items from the Orders table:

```python
try:
    # Search for a customer's favourite items
    response = client.query(
        TableName='Orders',
        KeyConditionExpression='CustomerId = :customer_id AND begins_with(SK, :sk)',
        ExpressionAttributeValues={
            ':customer_id': { 'S': '7970241400' },
            ':sk': { 'S': 'FAVOURITE#' }
        }
    )
    return response
except Exception as e:
    print(f"An error occurred: {str(e)}")
    return []
```

---

### **Best Practices When Updating Your DynamoDB Table Data Model**

Consider the following points:

* **Consider projecting only a subset of item attributes to the new index** – When adding a new GSI to your DynamoDB table, consider projecting only the attributes that you actually need. [Projection](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GSI.html#GSI.Projections) refers to attributes copied from the base table to the index. When creating a GSI, you can choose to project the table's key attributes, specific attributes, or all attributes from the base table. Projection to a GSI cannot be changed after the index has been created. If you need to evolve a data model of an existing GSI, you must create a new GSI. The attributes projected to the index affect the storage cost and write cost of the index.

* **Document changes to your table data model** – Document changes to your table's data model as your application features evolve. Include details and reasoning behind each change. This helps maintain consistency for data stored in DynamoDB.

* **Complete data integrity checks in your application** – DynamoDB does not maintain relationships between different entities stored in a table. You must maintain all entity relationships in your application. We recommend using [transactions](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/transactions.html) and [conditional writes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/WorkingWithItems.html#WorkingWithItems.ConditionalUpdate) when working with multiple related items to maintain data consistency and integrity. For example, use the TransactWriteItems command to ensure that when you create a new favorite entity in the Orders table, both the operation to update the order item as favorite and the operation to create the favorite entity are completed as a single transaction.

The following Python code example illustrates how to execute a TransactWriteItems command on the Orders table:

```python
try:
    # Execute the transaction
    response = client.transact_write_items(
        TransactItems=[
            # Item 1: Create a new favourite item
            {   
                'Put': {
                    'TableName': 'Orders',
                    'Item': {
                        'CustomerId': { 'S': '7970241400' },
                        'SK': { 'S': 'FAVOURITE#484295' },
                        'ItemId': { 'S': '484295' },
                        'ItemPrice': { 'S': '2.99' },
                        'ItemName': { 'S': 'Eggs' },
                        'ItemDescription': { 'S': 'Free Range Eggs' },
                        'ItemCategory': { 'S': 'Fresh' }
                    }
                }
            },
            
            # Item 2: Update an existing order's item type
            {
                'Update': {
                    'TableName': 'Orders',
                    'Key': {
                        'CustomerId': { 'S': '7970241400' },
                        'SK': { 'S': '2025-03-01#2121195' }
                    },
                    'UpdateExpression': 'Set #Items[0].Favourite = :Favourite',
                    'ExpressionAttributeNames': {
                        '#Items': 'Items'
                    },
                    'ExpressionAttributeValues': {
                        ':Favourite': { 'BOOL': True },
                        ':ItemId': { 'S': '484295' }
                    },
                    'ConditionExpression': '#Items[0].Id = :ItemId'
                }
            }
        ]
    )
    return response
except Exception as e:
    print(f"An error occurred: {e}")
    return []
```

In the example above, **UpdateExpression** sets the value of the Favourite attribute of the first element in the Items array to True.

---

### **Common Mistakes to Avoid**

Keep the following troubleshooting tips in mind:

* **Minimize the number of indexes on a table** – GSIs allow alternative access patterns for data in DynamoDB. However, each new index increases storage and write cost. Throughput provisioned for each GSI is independent of the base table. Therefore, write, update, or delete operations on the base table that affect items in the GSI will consume the GSI's write capacity. To optimize costs, consider projecting KEYS_ONLY to the GSI. This creates the smallest possible GSI, but your application may need to make two DynamoDB calls for the new access pattern. This is a tradeoff between storage/write costs and the cost of making additional read calls.

* **Avoid using DynamoDB for analytical access patterns** – DynamoDB is ideal for Online Transaction Processing (OLTP) use cases because access patterns are well-defined. Don't update your DynamoDB data model as your application evolves to include analytical access patterns. DynamoDB provides zero-ETL integration capabilities with many other AWS services specifically designed for analytics, including [Amazon OpenSearch Service](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-integration-opensearch.html), [Amazon Redshift](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/RedshiftforDynamoDB-zero-etl.html), and [Amazon Simple Storage Service (Amazon S3)](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/S3DataExport.HowItWorks.html).

---

### **Conclusion**

In this post, we showed you how to update your DynamoDB table's data model as your application requirements change. You can use the techniques discussed to update your DynamoDB table's data model without incurring application downtime.

Try applying these techniques to your use case, and share your feedback in the comments section.

---

### **About the Author**

**Imhoertha Ojior**

[Imhoertha](https://www.linkedin.com/in/iojior/) is a Solutions Architect at AWS in London. He is passionate about helping customers design distributed applications that require reliable database performance at any scale.