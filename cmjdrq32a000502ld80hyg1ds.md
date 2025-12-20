---
title: "How to Design a Scalable Notification System
(Email, Push, SMS)"
seoTitle: "Designing a Scalable Notification System"
seoDescription: "Learn how to build a scalable notification system that handles emails, push notifications, and SMS smoothly, without spamming users"
datePublished: Sat Dec 20 2025 03:58:21 GMT+0000 (Coordinated Universal Time)
cuid: cmjdrq32a000502ld80hyg1ds
slug: how-to-design-a-scalable-notification-system-email-push-sms
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1766203007435/f2656c27-4729-4147-bb12-9da5015e3d13.png
tags: software-development, system-architecture, system-design, notifications

---

## 1\. The Real Problem (Why This Matters)

A few years ago, we shipped a feature that sent notifications on every user action.

Everything worked fine…  
Until marketing sent a campaign to **1 million users**.

What happened next:

* Servers spiked
    
* Queues backed up
    
* Some users got **10 notifications**
    
* Others got **none**
    

The root cause wasn’t scale.

It was **design**.

> Notifications are easy to build — but hard to scale correctly.

---

## 2\. Why Notification Systems Are Deceptively Hard

At first glance, notifications look simple:

> “When X happens → send a message”

In production, they must handle:

* Multiple channels (Email, Push, SMS)
    
* Third-party rate limits
    
* Retries & failures
    
* Ordering & deduplication
    
* User preferences & unsubscribes
    
* Sudden traffic spikes (campaigns, alerts)
    

A poorly designed notification system can:

* Slow down core APIs
    
* Spam users
    
* Amplify outages instead of isolating them
    

---

## 3\. The Correct Mental Model

### ❌ Wrong

```plaintext
API → Send Email / Push / SMS
```

### ✅ Right

```plaintext
Event → Queue → Workers → Providers
```

**Golden Rule**

> 👉 Never send notifications directly from core business logic.

Notifications must be **asynchronous**, **decoupled**, and **fault-tolerant**.

---

## 4\. High-Level Architecture (Production Ready)

### Step 1: Event Producer

Core services emit **domain events**, not notifications.

Examples:

* `ORDER_PLACED`
    
* `PASSWORD_CHANGED`
    
* `PAYMENT_FAILED`
    

Properties:

* Lightweight
    
* Immutable
    
* Fire-and-forget
    

```plaintext
{
  "event": "ORDER_PLACED",
  "userId": "123",
  "orderId": "789"
}
```

Core services should not care **how** notifications are sent.

---

### Step 2: Message Queue (The Backbone)

Use a queue as a **shock absorber**.

Options:

* Kafka → high throughput
    
* SQS / PubSub → managed simplicity
    
* RabbitMQ → flexible routing
    

Why queues matter:

* Absorb traffic spikes
    
* Enable retries
    
* Prevent cascading failures
    
* Decouple producers from consumers
    

---

### Step 3: Notification Service

This service:

* Consumes events
    
* Fetches user preferences
    
* Decides **which channels** to use
    

Example:

```plaintext
ORDER_PLACED →
  Push notification
  Email receipt
```

Key responsibility:

> Decision-making, not delivery.

---

### Step 4: Channel-Specific Workers

Split workers by channel:

* Email Worker
    
* Push Worker
    
* SMS Worker
    

Why this works:

* Each provider has different rate limits
    
* Failures stay isolated
    
* Easier horizontal scaling
    
* Independent retry policies
    

---

### Step 5: External Providers

Examples:

* Email → SES, SendGrid
    
* Push → FCM, APNs
    
* SMS → Twilio
    

Rules:

* Respect provider rate limits
    
* Use exponential backoff
    
* Log provider failures separately
    
* Never block other channels
    

---

## 5\. Retries Without Spamming Users

Bad retry logic = angry users.

Use:

* **Idempotency keys**
    
* Retry limits (e.g. max 3 attempts)
    
* Dead Letter Queues (DLQ)
    

**Idempotency key example**

```plaintext
notification_id = userId + eventId + channel
```

If it already exists → **skip**.

This guarantees:

* No duplicate notifications
    
* Safe retries
    
* Predictable behavior
    

---

## 6\. User Preferences & Unsubscribes

Never hardcode notification rules.

Store preferences like:

```plaintext
{
  "marketing_emails": false,
  "order_updates": true,
  "push_enabled": true
}
```

Critical rule:

> ✅ Check preferences **before enqueueing**, not after sending.

This saves:

* Cost
    
* Provider calls
    
* User trust
    

---

## 7\. Scaling Strategy That Actually Works

What scales:

* Workers, not producers
    
* Consumers per channel
    
* Queue partitions (by userId)
    

Optimizations:

* Batch sends where supported
    
* Separate queues for critical vs non-critical notifications
    

Result:

* Horizontal scalability
    
* Predictable load
    
* Controlled costs
    

---

## 8\. A Real Production Fix

**Before**

* Notifications sent synchronously in APIs
    
* Traffic spikes slowed everything
    
* Retries caused duplicates
    

**After**

* Events pushed to Kafka
    
* Channel-specific worker pools
    
* Idempotency + retries
    

**Results**

* ✅ 0 duplicate notifications
    
* ✅ ~70% API latency reduction
    
* ✅ No outages during campaigns
    

---

## 9\. Key Takeaways

* Notifications are infrastructure, not features
    
* Never send notifications synchronously
    
* Queues absorb spikes and failures
    
* Split workers by channel
    
* Idempotency is non-negotiable
    
* Respect user preferences early
    
* Design for provider failure
    

---

## 🔥 Mini Design Challenge

Design notification flows for:

1. OTP login
    
2. Marketing campaigns
    
3. Order updates
    

Now decide:

* Which must be **real-time**
    
* Which can be **delayed**
    
* Which can be **dropped**
    

If you can answer this, you understand notification systems.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766203030381/ba653b78-9c74-4efb-b369-8ab39dd6c711.png align="center")