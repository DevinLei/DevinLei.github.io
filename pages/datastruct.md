---
layout: page
title: 数据结构与算法 系列文章
titlebar: 数据结构与算法
subtitle: <span class="mega-octicon octicon-flame"></span>&nbsp;&nbsp; 数据结构与算法 系列教程
menu: 数据结构与算法
css: ['blog-page.css']
permalink: /datastruct
keywords: 数据结构,时间复杂度,排序算法,查找
---

<div class="row">

    <div class="col-md-12">

        <ul id="posts-list">
            {% for post in site.posts %}
                {% if post.category=='datastruct'  or post.keywords contains '数据结构'or post.keywords contains '算法' %}
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