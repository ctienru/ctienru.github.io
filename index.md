---
layout: default
title: Home
---

<div class="container">
    <section class="hero">
        <h1>Tien Ru (Terri) Chen</h1>
        <p>Software engineer. I build backend services, search, and data integrations. This blog records the decisions behind <a href="https://podvein.com">Podvein</a>, a bilingual podcast search system.</p>
        <p><a href="https://podvein.com">Podvein</a> · <a href="https://github.com/ctienru">GitHub</a></p>
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
