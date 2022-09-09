---
layout: post
title: Jekylly Post
categories: [Code]
tags: [Study]
description: just remember
author: "Vince"
permalink: Jekyll
---
## 一些 Jekyll 的常用写法


[内部文章可Link]({% post_url 2018-11-13-Say %})

贴内部图片
![有帮助的截图]({{ site.url }}/img/elephant.jpg)

内部文件下载 [下载 PDF]({{ site.url }}/assets/aurora.pdf).

语法高亮
{% highlight ruby %}
SELECT 
      r.rolname, 
      ARRAY(SELECT b.rolname
            FROM pg_catalog.pg_auth_members m
            JOIN pg_catalog.pg_roles b ON (m.roleid = b.oid)
            WHERE m.member = r.oid) as memberof
FROM pg_catalog.pg_roles r
WHERE r.rolname NOT IN ('rds_iam',
                        'rds_replication','rds_superuser',
                        'rdsadmin','rdsrepladmin')
ORDER BY 1;
{% endhighlight %}