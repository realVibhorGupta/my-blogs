---
title: "Alerting and On-Call: Why Most Alerts Are Useless"
seoTitle: "Understanding Useless Alerts in On-Call Systems"
seoDescription: "Discover why most alerts fail to reflect real user pain and how SLO-based alerting can improve incident response and reduce alert fatigue"
datePublished: Mon Feb 16 2026 06:41:33 GMT+0000 (Coordinated Universal Time)
cuid: cmlot3dgh000802i86afg77fq
slug: alerting-and-on-call-why-most-alerts-are-useless
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1771223366279/ca5ed4e0-b68e-4465-90b4-efbf2bfedf8a.png
tags: aws, data-science, backend, google, system-design

---

Modern systems don’t fail because engineers miss dashboards.  
They fail because **alerts fire that don’t represent real user pain**.

Mature SRE teams (including those at **Google**) moved away from infrastructure-driven alerts to **SLO-driven alerting**.

This article explains how.

## The real problem with most alerting systems

Most teams alert on:

* CPU &gt; 80%
    
* Memory &gt; 75%
    
* Pod restarts
    
* Queue depth
    
* Error logs
    

These alerts answer:

> “Is something wrong with a component?”

But on-call engineers need an answer to:

> “Are users currently suffering?”

This mismatch is the root cause of:

* alert fatigue
    
* slow incident response
    
* unnecessary wake-ups
    
* real incidents being ignored
    

## The SRE model: start from the user

SLO-based alerting flips the direction:

```plaintext
User experience
     ↓
   SLI
     ↓
   SLO
     ↓
 Alerting
```

## First: understand SLI, SLO and SLA

### SLI – Service Level Indicator

An SLI is a **measurable signal of user experience**.

Examples:

* % of successful checkout requests
    
* API latency under 300 ms
    
* video start time under 2 seconds
    

Typical SLI formula:

```plaintext
good events / total events
```

Example:

```plaintext
successful HTTP responses / total HTTP requests
```

### SLO – Service Level Objective

An SLO is your **target** for that SLI.

Example:

```plaintext
99.9% of requests succeed over 30 days
```

SLOs are:

* internal engineering goals
    
* used for alerting and prioritization
    

### SLA – Service Level Agreement

An SLA is:

* a legal or business commitment
    
* includes penalties
    
* usually looser than SLOs
    

Important rule:

> You alert on SLOs — not SLAs.

## Why alerting on metrics fails

Consider this system:

```plaintext
Client → LB → API → Cache → DB
```

You alert on:

* DB CPU
    
* cache hit ratio
    
* API error rate
    
* container restarts
    

During a deploy:

* CPU spikes
    
* cache warms up
    
* error rate blips for 30 seconds
    

You get paged.

But users never noticed.

This is operational noise.

## The SLO-based alerting principle

You only page when:

> The system is actively burning user experience faster than allowed.

That is measured using **error budgets**.

## Error budget (the most important concept)

If your SLO is:

```plaintext
99.9% availability per month
```

Your error budget is:

```plaintext
0.1% allowed failure
```

For 30 days:

```plaintext
~43 minutes of bad user experience
```

This budget is:

* spendable
    
* measurable
    
* enforceable
    

## How alerts are derived from error budget

Instead of:

```plaintext
alert if error_rate > 1%
```

You alert on:

```plaintext
alert if error budget will be exhausted too fast
```

This answers:

> “If this continues, will we break our SLO soon?”

## The two SLO alerts you actually need

### 1\. Fast burn alert (page someone)

Used for active, severe outages.

Example logic:

* We are consuming error budget at 10× the normal rate
    
* We will burn the monthly budget in minutes
    

This is your real on-call alert.

### 2\. Slow burn alert (ticket, not page)

Used for:

* memory leaks
    
* slow dependency degradation
    
* creeping latency
    
* partial failures
    

Example:

* error budget will be exhausted in 2–3 days
    

This creates work, not panic.

## Visual model (ByteByteGo style)

```plaintext
Requests
   |
   v
+---------+
|  SLI    | → success ratio / latency
+---------+
     |
     v
+---------+
|  SLO    | → 99.9%
+---------+
     |
     v
+------------------+
| Error Budget     |
| remaining time   |
+------------------+
     |
     v
Fast burn → page
Slow burn → ticket
```

## What you should NOT alert on

Do not page on:

* CPU
    
* memory
    
* disk
    
* pod restarts
    
* leader elections
    
* queue depth
    

These are:

👉 diagnostics  
  
👉 dashboards  
  
👉 context

They are not paging signals.

## Where infrastructure alerts still matter

Infrastructure alerts are still useful when:

* they are clearly tied to user impact
    
* they directly predict SLO violations
    

Example:

* all DB replicas unreachable
    
* API error rate immediately spikes
    

In practice:

* infra alerts feed investigation
    
* SLO alerts drive escalation
    

## How mature teams implement this

A typical production setup looks like:

```plaintext
Services emit SLIs
        ↓
Metrics system
        ↓
SLO definitions
        ↓
Error budget evaluation
        ↓
Alert rules
```

Many teams implement this using tools like  
**Prometheus**  
  
and visualize SLOs in  
**Grafana**.

But the tooling is secondary.

The mental model is the real change.

## How SLO alerting reduces alert fatigue

Because:

* alerts represent real user pain
    
* alerts are rate-limited by error budget
    
* short harmless spikes do not trigger pages
    

Result:

* fewer alerts
    
* higher trust in alerts
    
* faster reaction when a page happens
    

## The hidden benefit: better product decisions

Error budgets allow engineering and product to negotiate safely.

Example:

> “Can we run this risky migration today?”

You check:

* remaining error budget
    

If you have plenty:

→ proceed  
  
If you are already burning:

→ delay

This is how mature SRE teams balance:

* reliability vs velocity
    

## Real-world example (Google SRE model)

The SRE practice described in the book  
**Site Reliability Engineering**  
  
formalized:

* user-focused SLIs
    
* SLOs as first-class objects
    
* error budgets as release gates
    
* alerting derived from SLO burn rate
    

This approach later influenced how many large companies design on-call operations.

## A concrete example (API service)

### SLI

```plaintext
successful requests with latency < 300ms
----------------------------------------
total requests
```

### SLO

```plaintext
99.9% over 30 days
```

### Alert

Fast burn:

* 5-minute window
    
* burn rate &gt; 14x
    

Slow burn:

* 1-hour window
    
* burn rate &gt; 2x
    

Only these two alerts page or notify.

Everything else becomes context.

## Common mistakes when adopting SLO alerting

### 1\. Choosing internal metrics as SLIs

Bad SLI:

* DB latency
    

Good SLI:

* API response time seen by users
    

### 2\. Defining too many SLOs

Every SLO creates operational cost.

Start with:

* the top user journeys only
    

### 3\. Setting unrealistic SLOs

99.999% looks good in slides.

It destroys:

* error budgets
    
* developer velocity
    
* mental health of on-call engineers
    

## Actionable checklist

When you design alerting for a service:

* Identify top 1–3 user journeys
    
* Define one SLI per journey
    
* Set realistic SLOs
    
* Compute error budget
    
* Create:
    
    * one fast-burn alert
        
    * one slow-burn alert
        
* Move all other alerts to dashboards
    

## Key takeaways

* Alerts should represent **user pain**, not system noise
    
* SLI → SLO → error budget → burn rate is the correct pipeline
    
* Alert fatigue is a symptom of metric-driven alerting
    
* SLO-based alerting creates healthier on-call rotations and faster recovery
    

### Difficulty

**Intermediate**

This model requires discipline, but once adopted, it fundamentally changes how your team experiences incidents.