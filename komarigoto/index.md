---
---

<style>
  .komarigoto-hero {
    padding: 6px 0 28px;
    border-bottom: 1px solid #e6e9ed;
    margin-bottom: 32px;
  }

  .komarigoto-hero h1 {
    margin: 0 0 14px;
    font-size: clamp(30px, 5vw, 42px);
    line-height: 1.3;
    letter-spacing: -0.02em;
  }

  .komarigoto-hero p {
    margin: 0;
    max-width: 650px;
    color: #626b76;
    font-size: 17px;
    line-height: 1.8;
  }

  .section-title {
    margin: 38px 0 16px;
    font-size: 21px;
    font-weight: 700;
    color: #20242a;
  }

  .article-grid {
    display: grid;
    gap: 14px;
  }

  .article-card {
    display: block;
    padding: 20px 22px;
    background: #fff;
    border: 1px solid #e4e7eb;
    border-radius: 14px;
    color: inherit !important;
    text-decoration: none !important;
    transition:
      transform 0.15s ease,
      box-shadow 0.15s ease,
      border-color 0.15s ease;
  }

  .article-card:hover {
    transform: translateY(-2px);
    border-color: #b8cee5;
    box-shadow: 0 8px 22px rgba(20, 40, 70, 0.08);
  }

  .article-card-title {
    margin: 0 0 8px;
    color: #075fb8;
    font-size: 18px;
    font-weight: 700;
    line-height: 1.45;
  }

  .article-card-excerpt {
    margin: 0;
    color: #626b76;
    font-size: 14px;
    line-height: 1.7;
  }

  .category-heading {
    display: flex;
    align-items: center;
    gap: 8px;
    margin: 40px 0 16px;
    font-size: 20px;
    font-weight: 700;
  }

  .category-heading::before {
    content: "";
    width: 4px;
    height: 20px;
    border-radius: 4px;
    background: #075fb8;
  }

  .empty-message {
    padding: 26px;
    border: 1px dashed #cfd5dc;
    border-radius: 14px;
    color: #69727d;
    text-align: center;
  }

  @media (max-width: 640px) {
    .komarigoto-hero {
      padding-top: 0;
    }

    .komarigoto-hero p {
      font-size: 16px;
    }

    .article-card {
      padding: 17px 18px;
    }

    .article-card-title {
      font-size: 17px;
    }
  }
</style>


<div class="komarigoto-hero">
  <h1>暮らしの小さな困りごと</h1>

  <p>
    日常の中には、多くの人には気づかれにくくても、
    誰かにとっては切実な「ちょっと困る」があります。
    そんな見過ごされやすい困りごとを集め、考えていきます。
  </p>
</div>


{% assign articles = site.pages
  | where_exp: "p", "p.path contains 'komarigoto/articles/'" %}

{% assign root_articles = articles
  | where: "dir", "/komarigoto/articles/"
  | sort: "path"
  | reverse %}


{% if root_articles.size > 0 %}

<div class="article-grid">

{% for article in root_articles %}
{% if article.title %}

<a class="article-card" href="{{ article.url | relative_url }}">

  <div class="article-card-title">
    {{ article.title }}
  </div>

  <p class="article-card-excerpt">
    {{ article.content
      | markdownify
      | strip_html
      | normalize_whitespace
      | truncate: 105 }}
  </p>

</a>

{% endif %}
{% endfor %}

</div>

{% endif %}


{% assign groups = articles
  | group_by: "dir"
  | sort: "name" %}


{% for group in groups %}

{% unless group.name == "/komarigoto/articles/" %}

{% assign folder_name = group.name
  | url_decode
  | remove_first: "/komarigoto/articles/"
  | remove: "/" %}


<div class="category-heading">
  {{ folder_name }}
</div>

<div class="article-grid">

{% assign folder_articles = group.items
  | sort: "path"
  | reverse %}

{% for article in folder_articles %}

{% if article.title %}

<a class="article-card" href="{{ article.url | relative_url }}">

  <div class="article-card-title">
    {{ article.title }}
  </div>

  <p class="article-card-excerpt">
    {{ article.content
      | markdownify
      | strip_html
      | normalize_whitespace
      | truncate: 105 }}
  </p>

</a>

{% endif %}

{% endfor %}

</div>

{% endunless %}

{% endfor %}


{% if articles.size == 0 %}

<div class="empty-message">
  記事はまだありません。
</div>

{% endif %}
