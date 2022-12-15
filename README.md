# fetch_ae_assessment
## First: Review Existing Unstructured Data and Diagram a New Structured Relational Data Model
<img width="529" alt="Screenshot 2022-12-13 at 6 05 12 PM" src="https://user-images.githubusercontent.com/50029982/207961792-a8292a34-2778-40f0-9381-410e2ac3304d.png">

## Second: Write a query that directly answers a predetermined question from a business stakeholder
#### When considering average spend from receipts with 'rewardsReceiptStatus’ of ‘Accepted’ or ‘Rejected’, which is greater?
#### When considering total number of items purchased from receipts with 'rewardsReceiptStatus’ of ‘Accepted’ or ‘Rejected’, which is greater?
Since there is no "Accepted" status, it is assumed to be "Finished". SQLite used as engine

```
SELECT r.rewardsReceiptStatus, ROUND(AVG(r.totalSpent), 2) AS average_spend, SUM(i.quantityPurchased) AS total_purchased
FROM receipts r
    LEFT JOIN rewardsReceiptItemList i
        ON i.receipt_id = r._id
WHERE r.rewardsReceiptStatus in ("FINISHED", "REJECTED")
GROUP BY r.rewardsReceiptStatus
```

## Third: Evaluate Data Quality Issues in the Data Provided
Please see notebook for process

## Fourth: Communicate with Stakeholders
```
Hi there! I'm Kenji from AE team and I was reviewing our database for an automation project and I have couple of quick questions:
1) Going over our `user` data, there are lots of duplicates that is bloating our warehouse - a quick select query will show you that most are duplicated at least once (please see attached).
Is there a reason for this? If not, there might be a bug in the pipeline upstream. I can try to find what's causing it in the first place and quickly clean that up.  
2) The documentation also showed that this table should have the `role` column to be 'CONSUMER' exclusively, yet they are 'customer' (plus, 'fetch-staff' can be found here). Is this intended?
If left uncorrected, we might run into some issues in the future with bloated data warehouse and incorrect reports. 
Thanks in advance!
```
