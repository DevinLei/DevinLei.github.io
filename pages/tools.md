---
layout: page
title: 开发工具 系列文章
titlebar: 开发工具
subtitle: <span class="mega-octicon octicon-flame"></span>&nbsp;&nbsp; 开发工具 系列教程
menu: 开发工具
css: ['blog-page.css']
permalink: /tools
keywords: 开发工具,IDEA
---

<div class="row">

    <div class="col-md-12">

        <ul id="posts-list">
            {% for post in site.posts %}
                {% if post.category=='开发工具'  or post.keywords contains '开发工具' or post.keywords contains 'IDEA' %}
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