---
layout: page
title: 人工智能 系列文章
titlebar: 人工智能
subtitle: <span class="mega-octicon octicon-flame"></span>&nbsp;&nbsp; 人工智能 系列教程
menu: 人工智能
css: ['blog-page.css']
permalink: /ai
keywords: 人工智能,AI,python
---

<div class="row">

    <div class="col-md-12">

        <ul id="posts-list">
            {% for post in site.posts %}
                {% if post.category=='AI'  or post.keywords contains '人工智能' or post.keywords contains 'AI' or post.keywords contains 'python' %}
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