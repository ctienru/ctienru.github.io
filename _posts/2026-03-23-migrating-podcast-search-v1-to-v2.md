---
layout: post
title: "How I'm Migrating Podcast Search From v1 to v2"
date: 2026-03-23
author: Terri
tags: [search, elasticsearch, podcast, devops]
---

The previous post covered why I'm rebuilding the search system. This one covers how I'm thinking about the cutover.

## The First Idea (and Why I Dropped It)

The obvious approach was to migrate on the same machine. Shut down v1, reindex everything, bring up v2.

I dropped it for one reason: v1's Elasticsearch heap was already at 85%. That was one of the reasons I was migrating in the first place. Reindexing 170K+ episodes on the same machine would almost certainly OOM.

And if something went wrong halfway through, there's no clean rollback path.

## The Actual Constraint

The real constraint wasn't technical complexity. It was this:

I needed to be able to test v2 fully before it touched any production traffic. And I needed rollback to be trivial if something broke after cutover.

Those two requirements ruled out in-place migration.

## Blue-Green

The approach: build v2 on a new VM, verify everything completely, then cut over with a DNS change. v1 keeps running and serving traffic the whole time.

If something breaks after the switch, rollback is another DNS change. v1 is still running—there's nothing to restore.

Rollback = one DNS change.

Since v2 is built on a separate machine, there's no need to keep anything backward-compatible with v1. No feature flags, no migration shims, no "support both modes" logic. Just write the new version cleanly.

The cost is running two VMs in parallel for a few days. For a side project, that's real money. But the safety margin is worth it.

## The Lesson

I spent a lot of time thinking about the new index design. But the migration strategy needed the same level of thought.

Think about the rollback path first. Everything else follows from there.

---

*This post is part of a series on building [PodVein](https://podvein.com), a semantic search engine for podcasts.*
