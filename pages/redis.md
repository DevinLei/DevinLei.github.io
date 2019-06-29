---
layout: page
title: redis 系列文章
titlebar: redis
subtitle: <span class="mega-octicon octicon-flame"></span>&nbsp;&nbsp; redis 系列教程
menu: redis
css: ['blog-page.css']
permalink: /redis
keywords: redis,缓存穿透,缓存雪崩,分布式锁
---

<div class="row">

    <div class="col-md-12">

        <ul id="posts-list">
            {% for post in site.posts %}
                {% if post.category=='redis'  or post.keywords contains 'Redis Boot'or post.keywords contains 'redis' %}
                <li class="posts-list-item">
                    <div class="posts-content">
                        <span class="posts-list-meta">{{ post.date | date: "%Y-%m-%d" }}</span>
                        <a class="posts-list-name bubble-float-left" href="{{ site.url }}{{ post.url }}">{{ post.title }}</a>
                        <span class='circle'></span>
                    </div>
                </li>
                {% endif %}
            {% endfor %}
        </ul> 

        <!-- Pagination -->
        {% include pagination.html %}

        <!-- Comments -->
       <div class="comment">
         {% include comments.html %}
       </div>
    </div>

</div>
<script>
    $(document).ready(function(){

        // Enable bootstrap tooltip
        $("body").tooltip({ selector: '[data-toggle=tooltip]' });

    });
</script>