---
title: "Idempotency Explained — How to Prevent Duplicate Actions in Distributed Systems"
seoTitle: "Understanding Idempotency in Distributed Systems"
seoDescription: "Learn the balance between normalization and denormalization in databases to prevent duplicate actions and ensure system performance and integrity"
datePublished: Sun Jan 04 2026 06:25:46 GMT+0000 (Coordinated Universal Time)
cuid: cmjzclftw000102li8hmh8vtx
slug: idempotency-explained-how-to-prevent-duplicate-actions-in-distributed-systems
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1767507888305/dc75dac7-a0b3-41e6-8d6a-9dd408c97970.png
tags: software-development, aws, java, javascript, data-science, system-design

---

## **The Day My Perfectly Normalized Database Became the Slowest Part of the System**

A few years ago, I built a booking system with textbook-perfect database design:

* Fully normalized tables
    
* No duplicate data
    
* Strict foreign keys
    
* Everything neat, clean, and relational
    

I was proud… until users started complaining:

> **“Why does the app feel slow?”**

Fetching a single booking required **7 JOINs**.  
My “clean” database wasn’t the problem — **my over-normalization was**.

That day I learned something important:

> **Databases need balance, not purity.**  
> The correct design is the one that serves the system, not the textbook.

---

## **📌 Why Understanding Normalization vs. Denormalization Actually Matters**

Most developers fall into two traps:

1. **Over-normalization** → Too many JOINs → Slow queries
    
2. **Over-denormalization** → Duplicate data → Update headaches
    

Real-world systems rarely live at the extremes — you almost always need a **hybrid**.

This guide will help you understand exactly when to use which.

---

# **What Is Normalization? (Simple Explanation)**

**Normalization = splitting data into smaller, related tables to remove redundancy.**

Example:

Instead of storing a user’s name in 10 tables, store it once in a `users` table and reference it using a `user_id`.

### **🎯 Why We Normalize**

* Avoid duplicate data
    
* Ensure consistency
    
* Reduce anomalies
    
* Make updates predictable
    
* Improve data integrity
    

---

## **When Normalization Works Best**

Normalization shines when **consistency matters more than read speed**, such as:

* Banking apps & financial systems
    
* Inventory & ERP systems
    
* CRMs with frequent updates
    
* User profiles
    
* E-commerce transactions
    
* Systems with heavy writes
    

### **Real Advantages**

* Cleaner, smaller tables
    
* No duplication errors
    
* Easier updates
    
* Better data integrity
    

### **Real Drawbacks**

* More JOINs
    
* Slower read operations
    
* Harder to query for reporting
    
* More complex schemas
    

---

# **What Is Denormalization? (Practical Definition)**

**Denormalization = intentionally storing redundant data to eliminate JOINs and speed up reads.**

Example:

Store the user’s name directly inside `bookings` instead of JOINing with `users`.

### **🎯 Why We Denormalize**

* Faster queries
    
* Fewer JOINs
    
* Better performance in dashboards, feeds, and reports
    

---

## **Where Denormalization Shines**

Ideal for **read-heavy systems**, like:

* Dashboards
    
* Analytics
    
* Feeds (news, social media)
    
* E-commerce product listings
    
* Search queries
    
* High-traffic APIs
    

### **Real Advantages**

* Lightning-fast reads
    
* Less stress on DB servers
    
* Simple queries (no JOIN chains)
    

### **Real Drawbacks**

* Duplicate data
    
* Higher storage cost
    
* Harder to update
    
* Potential inconsistencies
    

---

# **🔥 Real Case Study: How a Hybrid Approach Saved Our App**

In that booking system, I had:

* `users`
    
* `services`
    
* `bookings`
    
* `prices`
    
* `payments`
    
* `statuses`
    
* `logs`
    

Fetching one booking required **7 JOINs**.

On production, queries were taking **1.4 seconds**.  
That’s unacceptable for a dashboard that loads hundreds of bookings.

### **Solution: Add a Denormalized Summary Table**

I created a table called:

```plaintext
booking_summary
```

It stored:

* user\_name
    
* service\_name
    
* booking\_date
    
* price
    
* status
    
* created\_on
    

These values were **pre-joined and precomputed**.

### **Result?**

* Dashboard load time: **1.4s → 200ms**
    
* Server CPU: **40% → 22%**
    
* DB query load: **massively reduced**
    

This kept the core database normalized while giving us **fast reads where it mattered**.

---

# **🔥 How I Now Decide (Simple Rule)**

Here’s my rule of thumb for every new system:

### **1\. If data updates frequently → Normalize it**

Examples:

* User profiles
    
* Prices
    
* Inventory counts
    
* Transactions
    

### **2\. If data is read frequently → Denormalize it**

Examples:

* Dashboards
    
* Feeds
    
* Admin panels
    
* Search results
    

### **3\. If both read & write are heavy → Hybrid**

* Use normalized tables for source of truth
    
* Use summary tables/materialized views for fast reads
    

---

# **📌 Quick Copy-Paste Summary for Developers**

✔ Normalize when accuracy & consistency matter  
✔ Denormalize when speed matters more than purity  
✔ Too many JOINs = silent performance killer  
✔ Dashboards almost always need denormalization  
✔ Use precomputed summary tables for high-traffic endpoints  
✔ Don’t denormalize everything — only what’s needed

---

## **1️⃣5️⃣ Mini Challenge**

Pick one API you’ve built.

Ask:

* What happens if the request is sent twice?
    
* Will money, data, or side effects duplicate?
    
* Where can I add idempotency keys?
    

If you can’t answer confidently — you need idempotency.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767507903977/6d5edffb-bd10-4042-89c5-3422328155fd.png align="center")