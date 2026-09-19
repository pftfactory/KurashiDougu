---
---

# 暮らしの小さな困りごと

日常の中で、見過ごされやすい小さな困りごとを集めています。

{% assign articles = site.pages | where_exp: "p", "p.path contains 'komarigoto/articles/'" %}

{% assign root_articles = articles | where: "dir", "/komarigoto/articles/" | sort: "path" | reverse %}

{% for article in root_articles %}
{% if article.title %}
- [{{ article.title }}]({{ article.url | relative_url }})
{% endif %}
{% endfor %}

{% assign groups = articles | group_by: "dir" | sort: "name" %}

{% for group in groups %}
{% unless group.name == "/komarigoto/articles/" %}
{% assign folder_name = group.name | url_decode | remove_first: "/komarigoto/articles/" | remove: "/" %}

## {{ folder_name }}

{% assign folder_articles = group.items | sort: "path" | reverse %}
{% for article in folder_articles %}
{% if article.title %}
- [{{ article.title }}]({{ article.url | relative_url }})
{% endif %}
{% endfor %}

{% endunless %}
{% endfor %}
