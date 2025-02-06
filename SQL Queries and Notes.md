# Query 1: What are the top 5 brands by receipts scanned for most recent month?
'''
SELECT 
    IFNULL(b.name, "Brand Unknown") AS brand_name, i.barcode,
    SUM (i.quantityPurchased) AS timesBought
FROM 
    `stately-lodge-438101-s3.Fetch.itemizedReceipt` AS i
LEFT JOIN 
    `stately-lodge-438101-s3.Fetch.brand` AS b ON i.barcode = CAST(b.barcode AS STRING)
LEFT JOIN
    `stately-lodge-438101-s3.Fetch.receipt` AS r ON i._id = r._id
WHERE 
    EXTRACT(MONTH FROM r.dateScanned_converted) = 
    (SELECT EXTRACT(MONTH FROM DATE_SUB(DATE(MAX(dateScanned_converted)), INTERVAL 1 MONTH)) FROM `stately-lodge-438101-s3.Fetch.receipt`)
    AND i.barcode IS NOT NULL
GROUP BY 
    brand_name, i.barcode
ORDER BY 
    timesBought DESC
LIMIT 5;  -- Get the Top 5 brands with the highest number of transactions	
 '''
Notes: 
1.	“dateScanned” attributed converted from unix time to datetime format, hence the name “dateScanned_converted”.
2.	No receipts scanned in the most recent month (October 2024), so I have calculated the most recent month in the column and used that.
3.	Noticing that the most recent month did not have many receipts scanned and didn’t get the top 5 results, 
    hence went back one more month.
4.	These barcodes do not have a matching brand name to it – important for business to identify these.
5.	Left Join was used because there were no matching barcodes for the receipts scanned even after the assumption in 
    point number 3 leaving me with no results.
6.	Added all the quantity purchased for each item since each receipt can have more than one similar item purchased.
7.	Instead of keeping the brand name column null, I decided to replace it with “Brand Unknown”.
8.	I included the barcode column since I think it is important for the business to know and acknowledge them even 
    though there is no brand name for them since they were the top 5 barcodes with highest number of transactions.

# Query 3: When considering average spend from receipts with 'rewardsReceiptStatus’ of ‘Accepted’ or ‘Rejected’, which is greater?
'''
SELECT 
    rewardsReceiptStatus, round(AVG(totalSpent), 2) AS avgSpending
FROM 
    `stately-lodge-438101-s3.Fetch.receipt`
GROUP BY 
    rewardsReceiptStatus
ORDER BY 
    avgSpending DESC;
 '''
Notes:
1.	Assumed “Accepted” is the same as “Finished”.
2.	“Finished” status is greater when considering average spend from receipts.

# Query 4: When considering total number of items purchased from receipts with 'rewardsReceiptStatus’ of ‘Accepted’ or ‘Rejected’,
which is greater?
'''
SELECT 
    rewardsReceiptStatus, SUM(purchasedItemCount) AS itemPurchasedCount
FROM 
    `stately-lodge-438101-s3.Fetch.receipt`   
GROUP BY 
    rewardsReceiptStatus
ORDER BY 
    itemPurchasedCount DESC;
 '''
Notes:
1.	Assumed “Accepted” is the same as “Finished”.
2.	“Finished” status is greater when considering total number of items purchased from receipts.

# Query 5: Which brand has the most spend among users who were created within the past 6 months?
'''
SELECT 
    b.name AS brand_name, i.barcode,
    ROUND(SUM (i.finalPrice),2) as Amount -- sum of amount spent
FROM 
    `stately-lodge-438101-s3.Fetch.user` AS u
INNER JOIN 
    `stately-lodge-438101-s3.Fetch.receipt` AS r ON u.id = r.userId
INNER JOIN 
    `stately-lodge-438101-s3.Fetch.itemizedReceipt` AS i ON r._id = i._id
INNER JOIN
    `stately-lodge-438101-s3.Fetch.brand` AS b ON i.barcode = CAST(b.barcode AS STRING)
WHERE 
    u.createdDate_converted BETWEEN (
        SELECT TIMESTAMP(DATE_SUB(DATE(MAX(u.createdDate_converted)), INTERVAL 6 MONTH))
        FROM `stately-lodge-438101-s3.Fetch.user` AS u
    ) AND (
        SELECT TIMESTAMP(MAX(u.createdDate_converted))
        FROM `stately-lodge-438101-s3.Fetch.user` AS u
    ) 
GROUP BY 
    brand_name, i.barcode
ORDER BY 
    Amount DESC
LIMIT 5;  -- Get the brand with the highest spend
 '''
Notes:
1.	Since the data is from 2021, past 6 month is calculated from the latest date in the “createdDate_converted”. 
    Assumption was made to have results.
2.	“createdDate” attributed converted from unix time to datetime format, hence the name “createdDate_converted”.
3.	Used Inner join to show results which have a brand name and a barcode. There are a lot of brands with a 
    higher spend than $196.98 but do not have a brand name, only a barcode. It would be useful for the business to find them.
4.	From this result we can see that “Cracker Barrel Cheese” has the most spend, $196.88.

# Query 6: Which brand has the most transactions among users who were created within the past 6 months?
'''
SELECT 
    b.name AS brand_name, i.barcode,
    COUNT (r.dateScanned) as transactionCount -- Count of transactions
FROM 
    `stately-lodge-438101-s3.Fetch.user` AS u
INNER JOIN 
    `stately-lodge-438101-s3.Fetch.receipt` AS r ON u.id = r.userId
INNER JOIN 
    `stately-lodge-438101-s3.Fetch.itemizedReceipt` AS i ON r._id = i._id
INNER JOIN
    `stately-lodge-438101-s3.Fetch.brand` AS b ON i.barcode = CAST(b.barcode AS STRING)
WHERE 
    u.createdDate_converted BETWEEN (
        SELECT TIMESTAMP(DATE_SUB(DATE(MAX(u.createdDate_converted)), INTERVAL 6 MONTH))
        FROM `stately-lodge-438101-s3.Fetch.user` AS u
    ) AND (
        SELECT TIMESTAMP(MAX(u.createdDate_converted))
        FROM `stately-lodge-438101-s3.Fetch.user` AS u
    ) 
GROUP BY 
    brand_name, i.barcode
ORDER BY 
    transactionCount DESC
LIMIT 1;  -- Get the brand with the highest number of transactions
 '''
Notes:
1.	Since the data is from 2021, past 6 month is calculated from the latest date in the “createdDate_converted”. 
    Assumption was made to have results.
2.	“createdDate” attributed converted from unix time to datetime format, hence the name “createdDate_converted”.
3.	Used Inner join to show results which have a brand name and a barcode. There are a lot of brands with many more 
    transactions than 23 but do not have a brand name, only a barcode. It would be useful for the business to find them.
4.	Counted the number of times a brand appeared on a receipt and not the quantity of the item sold since the word 
    transaction was mentioned in the question and I assume one transaction is an entire receipt irrespective of the 
    times an item was bought.
5.	From this result we can see that “Tostitos” has the most transaction count, 23.
