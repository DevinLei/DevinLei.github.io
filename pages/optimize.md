---
layout: page
title: 性能调优 系列文章
titlebar: 性能调优
subtitle: <span class="mega-octicon octicon-flame"></span>&nbsp;&nbsp; 性能调优 系列教程
menu: 性能调优
css: ['blog-page.css']
permalink: /optimize
keywords: 性能调优,sql索引优化,代码优化,架构优化,JVM优化
---

<div class="row">

    <div class="col-md-12">

        <ul id="posts-list">
            {% for post in site.posts %}
                {% if post.category=='性能调优'  or post.keywords contains '性能优化'or post.keywords contains '索引优化' %}
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