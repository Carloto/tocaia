---
title: "SQL Injection in WHERE Clause — Hidden Data"
date: 2026-02-22T18:17:23-03:00
tags: ["write-up", "sql-injection", "portswigger"]
summary: ""
draft: false
---

## Description

The first lab of the SQL Injection Learning Path on PortSwigger Academy starts with a simple SQL injection vulnerability in a fictional online store. As stated in the lab description, the application contains a SQL injection vulnerability in the product category filter. The goal is to perform an attack that causes the application to display all the unreleased products.

## Reconnaissance

The website is a simple web store that sells some odd items. The home page shows a gallery of products, with a header entitled **Refine your search**.

![We Like To Shop Homepage](/tocaia/images/sqli-lab-01/home-unsolved.png)

From there you can select a category to filter the products. I have set up Firefox to proxy through Burp Suite, so I can intercept the requests. From there I sent the request to the Repeater tab and started to try to inject some SQL code.

## Exploitation
My first try was to simply add a SQL comment to the end of the query. I was inside the Accessories category, so I added `--` to the end of the query. It was partially successful, as it showed one new product, which I assume was an unreleased product. That was not enough, so I tinkered with the query a bit more.

I was too focused on trying to break the query that I forgot I could just add the `OR` operator to the query, and it would return all the hidden products. I tested the URL with `/filter?category=Accessories'+OR+released=0--` and it worked.

![Burp Suite](/tocaia/images/sqli-lab-01/burp.png)

This is the final result as seen on the website.

![Result](/tocaia/images/sqli-lab-01/result.png)

## References
Even though I was familiar with the basics of SQLi, the resources from PortSwigger Academy were a good refresher. The [What is SQL injection? (SQLi)](https://portswigger.net/web-security/sql-injection#what-is-sql-injection-sqli) article was particularly useful and I recommend it to anyone who is just starting out with SQLi.

## Takeaways
This is a very simple lab, but the implications of a vulnerability like this are very serious. A skilled attacker could use this to steal sensitive data, modify or delete data, or even take over the database.

As for Burp Suite, it will take a little bit of time to get used to. I've dabbled with Wireshark before, so I am not a complete stranger to packet analysis, but I still have a lot to learn.