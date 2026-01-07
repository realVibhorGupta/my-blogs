---
title: "Read vs. Write Amplification: The Hidden Tax on Database Performance"
seoTitle: "Database: Read vs. Write Amplification"
seoDescription: "Understanding read and write amplification's impact on database performance, latency, and hardware costs in B-Tree and LSM Tree storage architectures"
datePublished: Wed Jan 07 2026 05:04:55 GMT+0000 (Coordinated Universal Time)
cuid: cmk3k110k000502l5ewcdha9l
slug: read-vs-write-amplification-the-hidden-tax-on-database-performance
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1767761829666/79c433ce-e755-43dc-af84-356588cddcc8.png
tags: aws, java, javascript, backend, system-design

---

In system design, there is rarely a 1:1 relationship between what you ask for (logical operations) and what the storage engine actually does (physical operations).

This discrepancy is called **Amplification**. It is the hidden tax that dictates database performance, latency, and hardware costs (especially SSD wear).

The battleground for understanding this tradeoff is usually between two dominant storage architectures: the **B-Tree** (MySQL, PostgreSQL) and the **LSM Tree** (Cassandra, RocksDB).

Here is how they work.

---

### 1\. Write Amplification (WA)

Write Amplification occurs when the actual amount of physical data written to disk is a multiple of the logical data intended by the application.

**WA = (Total Data Written to Disk) / (Logical Data Written by User)**

If you update a 100-byte row, but the database eventually writes 10KB to the disk to persist that change, you have a Write Amplification factor of 100.

#### The Primary Culprit: LSM Tree Compaction

Log-Structured Merge (LSM) trees are optimized for extremely fast writing. They achieve this by treating all incoming writes as sequential **append-only** logs in memory, which are eventually flushed to disk as immutable files called SSTables.

But you cannot keep adding files forever. To reclaim space occupied by deleted or obsolete data, the database runs a background process called **Compaction**. This is where WA skyrockets.

Below is a visualization of the "Compaction Tax."

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767761732985/5417794c-b0e5-4971-ac1a-888578c8d561.png align="center")

A single piece of data might be merged and rewritten dozens of times as it moves deeper into the LSM tree structure over its lifetime.

**Why it matters:**

* **SSD Burnout:** Flash storage wears out after a certain number of write cycles. High WA kills SSDs faster.
    
* **Noisy Neighbors:** Compaction is I/O intensive. A massive compaction job can choke disk bandwidth, causing latency spikes for your live read queries.
    

---

### 2\. Read Amplification (RA)

Read Amplification occurs when the database must perform multiple physical disk checks to satisfy a single logical read request.

**RA = (Physical Reads) / (Logical Reads)**

If your app asks for "User ID: 123", and the database has to check four different files on disk to find it, your Read Amplification is 4.

#### The Primary Culprit: LSM Tree Layering

Because LSM trees never update files in place, updates to the same key are spread across different files created at different times.

To find the "current" truth, the database has to look at the newest data first and work backward.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767761793594/a1b15ab0-a1fd-4110-91dd-719e8350cb4d.png align="center")

The worse your compaction strategy (see above), the more fragmented your data is, and the higher your Read Amplification becomes.

**The Defense: Bloom Filters** To fight high RA, databases use **Bloom Filters**. This is a small, in-memory data structure that tells the database: *"The key is definitely NOT in this disk file"* or *"The key MIGHT be in this file."*

This allows the database to skip reading many files on disk, significantly lowering RA, though never quite hitting 1:1.

---

### Summary: The Great Trade-Off

There is no free lunch. You generally optimize for *either* read efficiency or write efficiency.

* **B-Trees (e.g., MySQL):** Optimize for **Low Read Amp**. They store data sorted on disk pages. Reads are fast and predictable (usually just 3-4 hops down the tree). *The Cost:* Random writes are slow because modifying 10 bytes requires rewriting an entire 16KB disk page (immediate high Write Amp).
    
* **LSM Trees (e.g., Cassandra):** Optimize for **High Write Throughput**. Writes are instantly appended to a log. *The Cost:* You pay the penalty later through heavy background compaction (Deferred Write Amp) and needing to check multiple files for reads (High Read Amp).
    

When designing a system, don't just look at generic benchmarks. Know your workload's read/write ratio, and pick the architecture whose "tax" you can afford to pay.