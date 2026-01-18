---
layout: default
title: Home
---

<div class="container">
    <section class="hero">
        <h1>Welcome to My Blog</h1>
        <p>Sharing thoughts, ideas, and experiences</p>
    </section>

    <section class="section">
        <h2>Recent Posts</h2>
        <div class="posts-grid">
            {% for post in site.posts limit:6 %}
            <article class="post-card">
                <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
                <p class="date">{{ post.date | date: "%B %d, %Y" }}</p>
                <p class="excerpt">{{ post.excerpt | strip_html | truncatewords: 30 }}</p>
                <a href="{{ post.url | relative_url }}" class="read-more">Read more →</a>
            </article>
            {% endfor %}
        </div>
    </section>
</div>
