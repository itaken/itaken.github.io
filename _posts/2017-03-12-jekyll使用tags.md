---
layout: post

title: "在jekyll中使用tags"
date: 2017-03-12 23:10:36 +0800

categories: post
tags: jekyll
---
1. 在根目录下, 新建文件 `tags.html`

2. 复制如下代码到 `tags.html` 文件中 (去掉内容中的`\`).

    ```html
    ---
    layout: page
    title: Tags
    ---
    <div class="tags-items">
      <div class="tags-items-list">
        {\% for tag in site.tags \%}
        <a href="#{\{ tag[0] | slugify \}}" class="post-tag">{\{ tag[0] \}}</a>
        {\% endfor \%}
      </div>
      <hr/>
      <div class="tags-items-section">
        {\% for tag in site.tags \%}
        <h2 id="{\{ tag[0] | slugify \}}">{\{ tag[0] \}}</h2>
        <ul class="tags-items-posts">
          {\% for post in tag[1] \%}
          <li>
              <a class="post-title" href="{\{ site.baseurl \}}{\{ post.url \}}">
                  {\{ post.title \}}<small class="post-date">{\{ post.date | date_to_string \}}</small>
              </a>
          </li>
          {\% endfor \%}
        </ul>
        {\% endfor \%}
      </div>
    </div>
    ```
3. 修改样式,保存. 效果如下
    ![jekyll tags]({{ site.url }}/assets/images/201703/12-01.png)

---
更多阅读
- [jekyll variables](https://jekyllrb.com/docs/variables/)
- [use-tags-and-categories-in-your-jekyll-based-github-pages](https://codinfox.github.io/dev/2015/03/06/use-tags-and-categories-in-your-jekyll-based-github-pages/)
- [tags.html](https://github.com/codinfox/codinfox-lanyon/blob/dev/blog/tags.html)
