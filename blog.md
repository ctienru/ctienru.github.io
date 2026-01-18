---
layout: default
title: Blog
---

<div class="container">
    <section class="section">
        <h2>All Posts</h2>
        <ul class="post-list">
            {% for post in site.posts %}
            <li>
                <article class="post-list-item">
                    <h2><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
                    <p class="date">{{ post.date | date: "%B %d, %Y" }}</p>
                    <p class="excerpt">{{ post.excerpt | strip_html | truncatewords: 50 }}</p>
                    <a href="{{ post.url | relative_url }}" class="read-more">Read more →</a>
                </article>
            </li>
            {% endfor %}
        </ul>
    </section>
</div>
