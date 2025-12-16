---
title: "Docker for Real Projects (Not Tutorial Examples)"
datePublished: Tue Dec 16 2025 14:48:55 GMT+0000 (Coordinated Universal Time)
cuid: cmj8p7bas000202ju91aefz0o
slug: docker-for-real-projects-not-tutorial-examples
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765896426651/46aa9422-0f0b-4ac9-9b16-9c192307e686.png
tags: software-development, docker, javascript, system-design

---

Containers Are Not About “Running Apps” — They’re About Surviving Production

---

Hook — A Real Problem I Faced

A few years ago, I deployed a Node.js API for a client.

It worked perfectly on my machine. Classic.

The moment it hit the server, everything broke:

missing dependencies

wrong Node version

OS-level differences

I spent 2 hours debugging problems that had nothing to do with the code.

That day, one lesson stuck permanently:

> Local environments lie. Containers don’t.

Docker stopped being “nice to have” and became non-negotiable for real projects.

---

Why This Matters (Beyond Tutorials)

Most developers learn Docker with:

“Hello World”

Nginx demo

Single Node container

That’s not reality.

Real systems include:

multiple services

environment variables

databases

caching layers

background workers

build optimization

CI/CD pipelines

Docker isn’t about containers. Docker is about:

consistency

reliability

reproducible builds

fast onboarding

predictable deployments

If your app touches production, Docker is infrastructure — not tooling.

---

Docker for Real Projects — The Parts That Actually Matter

1️⃣ Multi-Stage Builds (The Biggest Performance Win)

Most real projects ship 700–900MB images because dev dependencies leak into production.

That’s slow, expensive, and unnecessary.

Production-grade pattern:

# Stage 1 — Build

FROM node:18 AS builder WORKDIR /app COPY package\*.json ./ RUN npm ci COPY . . RUN npm run build

# Stage 2 — Runtime

FROM node:18-slim WORKDIR /app COPY --from=builder /app/dist ./dist COPY --from=builder /app/node\_modules ./node\_modules CMD \["node", "dist/index.js"\]

Why this matters:

smaller images

faster cold starts

fewer security risks

faster CI/CD

Multi-stage builds are not “optimization” — they are baseline.

---

2️⃣ .dockerignore — The Most Forgotten File

If you don’t have a .dockerignore, you are:

slowing builds

bloating images

leaking unnecessary files

Minimum viable .dockerignore:

node\_modules .git logs .env \*.md

Docker copies everything by default. Tell it what not to copy.

---

3️⃣ Docker Compose Is How Teams Actually Work

Real projects don’t run one service.

They run:

API

frontend

database

cache

queue workers

Production-realistic setup:

services: api: build: ./api ports: - "3000:3000" environment: DATABASE\_URL: postgres://user:pass@db:5432/app depends\_on: - db - redis

db: image: postgres:15 environment: POSTGRES\_PASSWORD: secret

redis: image: redis:latest

Now onboarding becomes:

docker compose up

No README rituals. No version mismatches. No “works on my machine.”

---

4️⃣ Separate Local, Staging & Production Configs

One Docker config for everything is a trap.

Use environment-specific files:

[docker-compose.dev](http://docker-compose.dev).yml

[docker-compose.prod](http://docker-compose.prod).yml

Why?

Production needs:

no hot reload

no file watchers

smaller images

real logging

different secrets

Local needs speed. Production needs stability.

They should never be identical.

---

5️⃣ Health Checks (The Silent Stability Booster)

Containers shouldn’t be “healthy” just because they started.

They should be healthy when they are ready.

healthcheck: test: \["CMD", "curl", "-f", "[http://localhost:3000/health](http://localhost:3000/health)"\] interval: 10s retries: 3

This prevents:

APIs starting before DB is ready

cascading failures

broken deployments

Health checks are cheap insurance.

---

Real Case — Docker Saved Our Team Time & Sanity

In a Flutter + Node.js + Redis + PostgreSQL project:

Onboarding required:

Node 18

PostgreSQL

Redis

Flutter SDK

Java

Android tools

Onboarding time: 3 days

We containerized everything.

New onboarding process:

git clone docker compose up

Onboarding time: 20 minutes

Deployments became 100% reproducible.

Docker didn’t just help. It removed friction entirely.

---

How I Personally Decide What Goes in Docker

Always containerize:

backend services

databases

caches

queues

workers

Sometimes containerize:

frontend (depends on workflow)

Never containerize blindly:

without build optimization

without environment separation

Docker is powerful — misuse it and you’ll hate it.

---

Final Takeaways (Save This)

Docker is infrastructure, not a tutorial tool

Multi-stage builds are mandatory

.dockerignore saves time & money

Docker Compose is for real teams

Separate dev & prod configs

Health checks prevent silent failures

If your app goes to production — Docker should already be there.