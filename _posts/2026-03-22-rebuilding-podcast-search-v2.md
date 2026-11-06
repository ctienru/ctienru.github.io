---
layout: post
title: "I'm Rebuilding the Podcast Search System"
date: 2026-03-22
author: Terri
tags: [search, elasticsearch, podcast]
excerpt: "I'm rebuilding the podcast search system from scratch. Not because I wanted to—because it broke."
---

> **Update (November 2026):** This post describes the v1 to v2 redesign on Elasticsearch from March 2026. Podvein has changed since then. The current version (v5) runs on OpenSearch, with a FastAPI search service and a Django API. See [podvein.com](https://podvein.com).

I'm rebuilding the podcast search system from scratch. Not because I wanted to—because it broke.

## What Went Wrong

The original design had everything in one Elasticsearch index: English, Traditional Chinese, Simplified Chinese, all mixed together. Multilingual embeddings, automatic Traditional↔Simplified Chinese conversion, hybrid search (BM25 + kNN + RRF). It looked great on paper.

Then two things happened.

Search results became unpredictable. I'd search in Traditional Chinese and get back a mix of Simplified Chinese and English. The CJK problem was especially bad—Traditional Chinese and Simplified Chinese share a lot of characters, so they score similarly even when the content is completely different. The results weren't wrong exactly. They were just unreadable.

The system also ran out of memory. 170K+ episodes, combined with dense embedding vectors and text indexes, maxed out the 8 GB VM. New episodes stopped coming in. The site went stale.

I thought it was a scaling problem. It wasn't. It was a design problem.

## What I Got Wrong

I built for coverage. The assumption was that cross-lingual search makes things better—more results, more languages, more flexibility.

But listeners don't need more languages. They need results they can actually read.

The issue wasn't that cross-lingual search existed. It was that it was the *default*. Every search was cross-lingual, whether the listener wanted that or not.

## The Fix

I'm splitting the single index into three: one for English, one for Traditional Chinese, one for Simplified Chinese. Each index has its own analyzer and its own embedding model.

I'm also removing the automatic Traditional↔Simplified Chinese conversion. It felt helpful, but it was one of the main sources of confusion—searching in Traditional Chinese and getting Simplified Chinese results isn't interchangeable, it's disorienting.

The new default is same-language only. Searching across both Chinese variants is opt-in: if selected, both indexes are queried and merged via RRF.

Precision first, recall on demand.

## What I Expect to Change

Results should be predictable. Search in Traditional Chinese, get Traditional Chinese results. That sounds obvious, but it wasn't before.

The memory problem should be gone. Each index grows independently, so I can manage and cap them separately. New episodes should start flowing in again.

And per-language tuning becomes possible. Before, adjusting ranking for one language would affect all others. Now each language is independent—I can use a Chinese-specific embedding for Chinese, tune BM25 weights separately, and so on.

## The Lesson

Don't optimize the problem I *think* listeners have. Optimize the one they *actually* have.

I was optimizing for multilingual coverage. The real problems were readability and reliability. One redesign should fix both.

---

*This post is part of a series on building [PodVein](https://podvein.com), a semantic search engine for podcasts.*
