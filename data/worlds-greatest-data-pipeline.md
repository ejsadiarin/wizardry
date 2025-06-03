---
date: 2025-06-04T00:13
title: World's Greatest Data Pipeline
tags: 
    - Data
---
<!-- 2025-06-04-0013 (June 04, 2025 12:13:34 AM) -->

# World's Greatest Data Pipeline

`world's greatest data pipeline`

It only seems new to some people because data engineering wasn't a specialized discipline until Hadoop was popularized.. Before that it was just a standard function of I.T.

If you pay attention to the foundations of a lot of data engineering terminology it goes back to the the unix mainframe days. We call them data pipelines because we use to pipe | one output to the next input. Data engineering is arguable the oldest common form of development given it was far more common for a mainframe user to write a data pipeline than a program in cobol/c/etc.

For example this is a data pipeline..

```bash
join -t',' -1 1 -2 1 <(cat sales\_\*.csv | awk 'NR==1 || FNR>1' | sed 's/\[\[:space:\]\]\*,\[\[:space:\]\]\*/,/g' | tr '\[:upper:\]' '\[:lower:\]' | sort -t',' -k1,1) <(cat customers\_\*.csv | awk 'NR==1 || FNR>1' | sed 's/"//g; s/\[\[:space:\]\]\*,\[\[:space:\]\]\*/,/g' | tr '\[:upper:\]' '\[:lower:\]' | sort -t',' -k1,1) | awk -F',' 'NR==1{print} NR>1{gsub(/\^\[\[:space:\]\]+|\[\[:space:\]\]+$/,"",$0); if($3 \~ /\^\[0-9\]+(\\.\[0-9\]+)?$/ && $5 != "") print}' | tee >(head -n 1) >(tail -n +2 | sort -t',' -k3,3nr) | awk '!seen\[$0\]++'
```

1. Loads sales and customer CSV files
2. Cleans both datasets (removes quotes, normalizes whitespace, converts to lowercase)
3. Joins them on customer ID (column 1)
4. Filters for valid records (numeric sales amount in column 3, non-empty column 5)
5. Sorts by sales amount (highest first)
6. Removes duplicates
7. Output: A cleaned, joined dataset of customers with their sales, sorted by sales value.

One of the really best things about old school data pipelines was for the most part you could process far more data than you ram would allow because you processing data on the line level. You want to process a PB of data no problem just as long as you managed your resources you could stream endless amount of data. It's read off the disk line by line and goes through the pipeline and you're barely using any resources.. 

If you don't know about Unix/linux OS level data pipelines, I highly recommend learning them. You'd be surprised but when I get into serious problems it's usually a terminal one liner with awk and other unix utilities that I bails me out and fixes the issue. Need to clean out the bad records from a single 1TB text based file and you can't load it into Spark or any other data processing engine.. that's when I goto the OS level pipelines.

---

ref: https://www.reddit.com/r/dataengineering/s/MZu2R7XnEb
 and https://www.reddit.com/r/dataengineering/s/17h8AAYM3N
