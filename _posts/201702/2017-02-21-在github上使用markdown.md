---
layout: post

title: "在github上使用markdown写笔记"
date: 2017-02-21 17:03:57 +0800

categories: post
tags: [markdown, github]
---
# markdown

1. [mastering-markdown](https://guides.github.com/features/mastering-markdown/)

    ```markdown
    1. Item 1
    1. Item 2
    1. Item 3
       1. Item 3a
       1. Item 3b
    ```
    1. Item 1
    1. Item 2
    1. Item 3
       1. Item 3a
       1. Item 3b

1. [write-in-markdown](http://programminghistorian.org/new-lesson-workflow#write-in-markdown)

    ```markdown
    <figure>
        <a href="../images/LESSON-SLUG/IMAGE-FILENAME">
           <img src="../images/LESSON-SLUG/IMAGE-FILENAME" alt="Caption to image">
        </a>
        <figcaption>
            Caption to image
        </figcaption>
    </figure>
    ```
    <figure>
        <a href="{{ site.url }}/assets/images/201702/14-02.png">
           <img src="{{ site.url }}/assets/images/201702/14-02.png" alt="Caption to image">
        </a>
        <figcaption>
            Caption to image
        </figcaption>
    </figure>

1. [代码高亮](https://help.github.com/articles/creating-and-highlighting-code-blocks/)

    ```markdown
    \```ruby
    require 'redcarpet'
    markdown = Redcarpet.new("Hello World!")
    puts markdown.to_html
    \```
    ```

    ```ruby
    require 'redcarpet'
    markdown = Redcarpet.new("Hello World!")
    puts markdown.to_html
    ```

---
# kramdown

- [kramdown Syntax](https://kramdown.gettalong.org/syntax.html)

    ```markdown
    Hello        {#id}
    -----

    # Hello      {#id}

    # Hello #    {#id}
    ```

    Hello        {#id}
    -----

    # Hello      {#id}

    # Hello #    {#id}

- [Example of how to embed a Gist on GitHub Pages using Jekyll.](https://gist.github.com/benbalter/5555251)

    ```markdown
    {\% gist 5555251 gist.md \%}
    ```

    {% gist 5555251 gist.md %}
