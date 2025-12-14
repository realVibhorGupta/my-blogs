---
title: "Efficient Pagination Without Killing Performance"
datePublished: Sun Dec 14 2025 07:22:16 GMT+0000 (Coordinated Universal Time)
cuid: cmj5ed7vu000302jp3azsbj9w
slug: efficient-pagination-without-killing-performance
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765696834606/fa72a5e7-60ba-481a-9e0f-3ab728f76c1a.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1765696888737/e9000bcb-e5d7-402d-8db4-1734b6237df2.png
tags: software-development, javascript, software-architecture, databases

---

## 🚨 The Night Pagination Broke My API

A few months ago, a client texted me at midnight:

> **“Bro… the product listing API takes 14 seconds. Users are uninstalling.”**

I checked the query.

```plaintext
SELECT * 
FROM products 
ORDER BY id 
LIMIT 20 OFFSET 900000;
```

The database had **10 million rows**, **no proper index**, and relied on **OFFSET-based pagination**.

Every “Next Page” click scanned and discarded hundreds of thousands of rows.

That night, I rewrote the pagination logic.

### The result:

* ⏱ **14s → 120ms**
    
* 💰 **40% lower server cost**
    
* 😴 No more midnight panic messages
    

That’s when pagination stopped being a UI concern for me — and became **a backend performance strategy**.

---

## Why Pagination Matters More Than You Think

Pagination looks harmless.

But bad pagination can:

* Get slower with every page
    
* Trigger full table scans
    
* Burn CPU and memory
    
* Increase cloud bills
    
* Cause API timeouts
    
* Kill UX and SEO
    

Good pagination can:

* Keep response times constant
    
* Handle millions of records
    
* Reduce database load
    
* Scale cleanly under traffic
    

👉 **Pagination is not** `page + limit`.  
It’s a **data access pattern**.

---

## How Databases Actually Handle Pagination (Mental Model)

Think of a database like a huge sorted notebook.

* **OFFSET pagination** = start from page 1 and flip pages until you reach page 94
    
* **Cursor pagination** = open the notebook exactly where you left off
    

One scales.  
The other dies quietly in production.

---

## Pagination Techniques That Actually Work

---

### ❌ A. OFFSET–LIMIT Pagination (The Default Mistake)

```plaintext
SELECT * 
FROM products 
ORDER BY id 
LIMIT 20 OFFSET 100000;
```

### What really happens:

* DB scans **100,020 rows**
    
* Discards **100,000**
    
* Returns **20**
    

### Problems:

* Performance degrades linearly
    
* Deep pages become unusable
    
* DB CPU spikes under load
    

### Use ONLY when:

* Dataset &lt; 50k rows
    
* Internal tools
    
* Performance is irrelevant
    

---

### ✅ B. Keyset (Cursor) Pagination — The Real Fix

Instead of page numbers, use **position**.

```plaintext
SELECT * 
FROM products
WHERE id > last_seen_id
ORDER BY id
LIMIT 20;
```

### Why it works:

* Uses index efficiently
    
* No skipped rows
    
* Constant-time performance
    

### Used by:

* Facebook
    
* Instagram
    
* YouTube
    
* Amazon
    
* Twitter
    

If your app has more than **100k rows**, this should be your default.

---

### ✅ C. Seek Pagination for Sorted Feeds

Perfect for timelines and feeds.

```plaintext
SELECT *
FROM posts
WHERE created_at < last_seen_timestamp
ORDER BY created_at DESC
LIMIT 20;
```

### Ideal for:

* Infinite scrolling
    
* Activity feeds
    
* Notifications
    
* Social timelines
    

Fast, predictable, and scale-proof.

---

### ⚡ D. Hybrid Pagination (OFFSET + Cursor)

Sometimes users want:

> “Jump to page 94”

Pure cursor pagination doesn’t support that well.

**Hybrid approach:**

1. Fetch only IDs using OFFSET
    
2. Use cursor for actual data fetch
    

Result:

* 50–70% less DB load
    
* Stable UX
    
* No deep-page slowness
    

---

### 🚀 E. Cache the Pages That Matter

Reality check:

* Page 1 gets **80% of traffic**
    

Cache it.

```plaintext
const cacheKey = `products_page_1`;
const cached = await redis.get(cacheKey);

if (cached) return cached;
```

Use:

* Redis
    
* Cloudflare
    
* In-memory cache
    

Caching + cursor pagination = serious performance gains.

---

## Real Case: Fixing Pagination in a Large E-commerce App

**System:**

* 4 million products
    
* Users demanded page numbers
    
* OFFSET-based pagination
    

### Problem:

* Page 94 = OFFSET 1.8 million
    
* Query time: **6–8 seconds**
    

### Solution:

✔ Default browsing → cursor pagination  
✔ Page jump → cached precomputed offsets  
✔ Proper indexing on `id`

### Result:

* ⏱ **8s → 200ms**
    
* 📉 **30% DB CPU reduction**
    
* 😍 No UX compromise
    

---

## How I Choose Pagination (Simple Rule)

| Scenario | Use This |
| --- | --- |
| Small dataset | OFFSET |
| Large dataset | Cursor / Keyset |
| Feeds & timelines | Seek pagination |
| Page jumps required | Hybrid |
| High traffic pages | Cache |

---

## Final Takeaways (Copy-Paste Friendly)

* OFFSET pagination gets slower the deeper you go
    
* Cursor pagination is constant-time and scalable
    
* Seek pagination is perfect for feeds
    
* Cache page 1 aggressively
    
* Hybrid models solve UX + performance
    
* Indexing is mandatory for fast pagination
    

---

## Mini Challenge

Run this on your slowest listing API:

```plaintext
EXPLAIN ANALYZE
```

Replace OFFSET pagination with cursor pagination.

Watch query time drop.  
That feeling?  
That’s production-grade backend engineering.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765696855557/6fef7b48-cdc6-438c-b305-5db1d3af1785.png align="center")