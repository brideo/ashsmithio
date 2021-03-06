---
title: 'Quick tip: Change Magento default increment ID for orders &#038; invoices'
author: Ash Smith
layout: post
share: true
permalink: /2012/12/quick-tip-change-magento-default-increment-id-for-orders-invoices/
categories:
  - magento1
---
Sometimes you will be required to change the default increment ID&#8217;s of orders, invoices, credit memos & shipments. To do this is surprisingly simple, yet cannot be done via the backend Admin area without an extension. Today I&#8217;ll show you how simple it is to change these numbers with some simple SQL queries to run on your database.

The table in question is eav\_entity\_store. Which has the last used increment id for each of the records we want to change. Knowing which is which is relatively easy to find out when you know where to look. In order to find out which is which you only need to take the entity\_type\_id and look at the records contained within eav\_entity\_type. From there you can spot which record corresponds  to the records within eav\_entity\_store.

To help you out, here are the records from a Magento 1.7.0.2 install I have. I can also verify that the same ID&#8217;s are in Magento 1.6.0.2 and 1.5.0.1 installs.

*   5 = Order
*   6 = Invoice
*   7 = Credit Memo
*   8 = Shipment

### Creating our SQL queries

With this knowledge we can apply this to creating our simple SQL update query to update our order numbers!

{% gist 4279368 update_order_numbers_1.sql %}

Okay, so that&#8217;s all great. But have you noticed your order numbers still begin with the number one? That&#8217;s because there is a prefix added to your order numbers. To change this, simply change the increment_prefix column in the same table. So let&#8217;s update our SQL

{% gist 4279368 update_order_numbers_2.sql %}

So, what if we don&#8217;t currently have any records in the eav\_entity\_store table, like in a clean fresh install with no orders then the above commands won&#8217;t work because we can&#8217;t update records that don&#8217;t yet exist. That means we need to insert them. So simply replace the UPDATE directive with INSERT.

There you have it, it&#8217;s as easy as that! You can now easily update order numbers, invoice numbers etc with these simple SQL queries.

Let me know in the comments if this has helped you, or if you have any other tips like these that help you.
