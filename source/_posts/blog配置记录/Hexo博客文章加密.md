---
title: Hexo博客文章加密
abbrlink: c30e6297
---
# Hexo博客文章加密

#### 前言

平时开发过程中遇到的一些问题，我都会整理到文档中。有些感觉不错的，会二次整理成文章发布到我的博客中。但是有些文章如果存在隐私内容，或者不打算公开的话，就不能放在博客中了。

我的博客是使用 `Hexo` 来搭建的，并不能设置某些文章不可见。但如果不在电脑旁或者出门没有带电脑又想要查看一下之前记录的内容，就很不方便了。

我也尝试在 `github` 上去找一些可以设置账户的开源的博客框架，但测试过一些后发现并没有符合自己需求的，而自己开发却没有时间。

思来想去，就想看看有没有插件能够实现 `Hexo` 博客的加密操作。最终让我找到了一款名为 `Hexo-Blog-Encrypt` 的插件。

为了防止以下的修改可能出现版本差异，这里我先声明我使用的 `Hexo` 版本信息：

```
hexo: 4.2.1
hexo-cli: 3.1.0
next theme version: 7.8.0+a7a948a
hexo-blog-encrypt: "^3.1.6"
```

------

#### 插件安装

```
npm install --save hexo-blog-encrypt
```

#### 快速使用

该插件的使用也很方便，这里我仅作简单介绍，详细的可以查看官方文档。 [D0n9X1n/hexo-blog-encrypt: Yet, just another hexo plugin for security.](https://github.com/D0n9X1n/hexo-blog-encrypt)

要为一篇文章添加密码查看功能，只需要在文章信息头部添加 `password` 字段即可：

```
---
title: hello world
date: 2021-04-13 21:18:02
password: hello
---
```

------

#### 全局加密配置

分别为每篇文章设置密码，虽然很灵活，但是配置或者修改起来非常麻烦。为此，可以通过设置统一配置来实现全局加密。

通过添加指定 `tag` 的方式，可以为所有需要加密的文章添加统一加密操作。只需要在需要加密的文章中，添加设置的 `tag值` 即可。

在Hexo主配置文件 `_config.yml` 中添加如下配置：

```
# Security
encrypt: # hexo-blog-encrypt
  silent: true
  abstract: 这是一篇加密文章，需要密码才能继续阅读。
  message: 当前文章暂不对外可见，请输入密码后查看！
  tags:
  - {name: private, password: hello}
  wrong_pass_message: 抱歉，您输入的密码错误，请检查后重新输入。
  wrong_hash_message: 抱歉, 当前文章不能被校验, 不过您还是可以看看解密后的内容。
```

之后，需要清除缓存后重新生成 `hexo clean && hexo s -g`。

其中的 `tag` 部分：

```
tags:
- {name: private, password: hello}
```

表示当在文章中指定了 `private` 这个 `tag` 后，该文章就会自动加密并使用对应的值 `hello` 作为密码，输入密码后才可查看。

相应的文章头部设置：

```
---
title: Password Test
date: 2019-12-21 11:54:07
tags:
    - private
---
```

#### 在全局加密配置下禁用某些文章的加密

可能有这样的情况，属于 `private` 标签下的某篇文章在一段时间内想要开放访问。如果在描述中加上密码提示： `当前文章密码为xxx，请输入密码后查看` ，来让用户每次查看时都要先输入密码后再查看，这样的操作又会给访客带来不便。

这时可以单独设置允许某篇文章不设置密码。

只需要在使用 `加密tag` 的前提下，结合 `password` 来实现即可。在博客文章的头部添加 `password` 并设置为 `""` 就能取消当前文章的 `Tag` 加密。

相应的设置示例如下：

```
---
title: No Password Test
date: 2019-12-21 11:54:07
tags:
    - private
password: ""
---
```

#### 在全局加密配置下设置非全局密码

在全局加密配置下，我们可以通过设置多个 `加密tag` 来为多篇不同类型的文章设置相同的查看密码：

```
tags:
- {name: private, password: hello}
- {name: jiami, password: world}
- {name: 加密, password: jiesuo}
```

那么可能有这样的场景：

属于 `private` 标签下的某篇文章想要设置成不一样的密码，防止用户恶意通过一个密码来查看同标签下的所有文章。此时，仍可以通过 `password` 参数来实现：

```
---
title: Password Test
date: 2019-12-21 11:54:07
tags:
    - private
password: "buyiyang"
---
```

说明：

该文章通过tag值 `private` 做了加密，按说密码应该为 `hello` ，但是又在信息头中设置了 `password` ，因为配置的优先级是 `文章信息头 > 按标签加密`，所以最后的密码为 `buyiyang` 。

------

#### 解密后目录不显示

在为某些文章设置了 **加密后查看** 之后，不经意间发现这些文章的目录在解密后却不显示了。

#### 探究原因

从插件的 `github issues` 中我找到了相关的讨论：

- [解密后目录不会更新 · Issue #16 · D0n9X1n/hexo-blog-encrypt](https://github.com/D0n9X1n/hexo-blog-encrypt/issues/16)

原因：

> 加密的时候，`post.content` 会变成加密后的串，所以原来的 `TOC` 生成逻辑就会针对加密后的内容。
> 所以这边我只能把原来的内容存进 `post.origin` 字段。

找到文件 `themes/next/layout/_macro/sidebar.swig` ，编辑如下部分：

[![20210418165143](https://gitee.com/leafney/blogimage/raw/master/blog/20210418165143.png)](https://gitee.com/leafney/blogimage/raw/master/blog/20210418165143.png)

[20210418165143](https://gitee.com/leafney/blogimage/raw/master/blog/20210418165143.png)



插件 `hexo-blog-encrypt` 对文章内容进行加密后，会将原始文章内容保存到字段 `origin` 中，当生成 `TOC` 时，我们可以通过 `page.origin` 来得到原始内容，生成文章目录。

相应的代码为：

```
<aside class="sidebar">
  <div class="sidebar-inner">

    {%- set display_toc = page.toc.enable and display_toc %}
    {%- if display_toc %}

      {%- if (page.encrypt) %}
        {%- set toc = toc(page.origin, { class: "nav", list_number: page.toc.number, max_depth: page.toc.max_depth }) %}
      {%- else %}
        {%- set toc = toc(page.content, { class: "nav", list_number: page.toc.number, max_depth: page.toc.max_depth }) %}
      {%- endif %}

      {%- set display_toc = toc.length > 1 and display_toc %}
    {%- endif %}

    <ul class="sidebar-nav motion-element">
```

修改完成后，执行 `hexo clean && hexo s -g` 并重新预览。

效果如下：

[![20210418165529](https://gitee.com/leafney/blogimage/raw/master/blog/20210418165529.png)](https://gitee.com/leafney/blogimage/raw/master/blog/20210418165529.png)

[20210418165529](https://gitee.com/leafney/blogimage/raw/master/blog/20210418165529.png)



不过，这样的效果貌似不是我想要的。我理想中的效果应该是：

- 当文章加密后，访客只能看到侧边栏中的 `站点概览` 部分，不需要看到 `文章目录` 部分。
- 当文章解密后，访客则可以看到 `站点概览` 和 `文章目录` 两部分。

而现在加密后的文章未解密之前也可以看到 `文章目录` ，虽然该目录不可点击。

当然，如果你不是很介意，那么到这里就可以结束了。如果你和我一样有一些 **追求完美的强迫症** 的话，我们继续。

##### 如何优化

查看了 `hexo-blog-encrypt` 相关的 `issues` ，我找到了一种 **折中** 的解决方法。

从 issue [Archer主题解密后TOC依旧不显示（已按手册修改）](https://github.com/D0n9X1n/hexo-blog-encrypt/issues/67#issuecomment-463893408) 中我们可以知道：

我们可以在文章加密的前提下，通过将目录部分加入到一个 `不可见的div` 中来实现 `隐藏目录` 的效果。在源码中的 [hexo-blog-encrypt/lib/hbe.js](https://github.com/D0n9X1n/hexo-blog-encrypt/blob/479ccd4cf522adc8f667cfa06290f057a219cb88/lib/hbe.js#L207) 部分我们也可以看到，解密后通过设置 `id` 值为 `toc-div` 的元素为 `display:inline` 来控制显示隐藏。

```
{%- if (page.encrypt) %}
  <div id="toc-div" style="display:none">
{%- else %}
  <div id="toc-div">
{%- endif %}

xxx这里是目录部分xxx

</div>
```

对文件 `themes/next/layout/_macro/sidebar.swig` 修改后的代码如下：

```
<!--noindex-->
<div class="post-toc-wrap sidebar-panel">
  {%- if (page.encrypt) %}
    <div id="toc-div" style="display:none">
  {%- else %}
    <div id="toc-div">
  {%- endif %}

    {%- if display_toc %}
    <div class="post-toc motion-element">{{ toc }}</div>
    {%- endif %}
  
  </div>
</div>
<!--/noindex-->
```

但这种方法并不是完全的加密，而是采用 `障眼法` 的方式，通过查看html源文件还是可以看到目录内容的，只是不显示罢了。

对于这个问题，`hexo-blog-encrypt` 插件的作者也作了说明：[next 主题内没有 article.ejs 文件【TOC 相关】 · Issue #162 · D0n9X1n/hexo-blog-encrypt](https://github.com/D0n9X1n/hexo-blog-encrypt/issues/162)

##### 只好妥协

因为该插件中目前只有一个参数 `page.encrypt` 可以用来判断当前的文章是否进行了 **加密处理** ，而不能获知该文章当前是处于 **加密后的锁定** 状态，还是处于 **加密后的解锁** 状态。如果再有一个参数结合起来一起处理就好了。

所以，目前只能在解锁前隐藏目录，解锁后再显示目录。但在解锁前目录区域还是会展开，只是没有内容显示罢了。

------

#### 让加密文章显示加密提示

类似于我的博客文章列表中的 `文章置顶` 的提示效果，考虑在文章列表中对加密的文章增加类似的 `加密` 提示信息。

上面对于文章的加密处理，一方面是在 `配置文件` 中添加的 `tag` 全局配置，另一方面是在单个 `md源文件` 中添加的 `password` 参数。所以我们需要对这两种情况分别做处理。

##### 对于password参数的情况

针对于 `password` 字段，参考获取其他字段的方法，比如获取标题用 `post.title` ，获取置顶用 `post.top` ，那么获取 `password` 就是 `post.password` 了。

可以参考我之前添加置顶提示信息的操作，对文件 `themes/next/layout/_macro/post.swig` 的修改如下：

```
{# 加密文章添加提示信息-for password #}
{%- if post.password %}
  <span class="post-meta-item">
      <span class="post-meta-item-icon">
          <i class="fas fa-lock"></i>
      </span>
      <span class="post-meta-item-text">
          <font color='#FD7E13'>[加密]</font>
      </span>
  </span>
{%- endif %}
```

##### 对于tag标签的情况

针对于 `tag` 标签的获取，可以从文件 `themes/next/layout/_macro/post.swig` 中找到类似的处理方法：

```
{%- for tag in post.tags.toArray() %}
    <a href="{{ url_for(tag.path) }}" rel="tag">{{ tag_indicate }} {{ tag.name }}</a>
{%- endfor %}
```

即可以用最简单的 **遍历法** 来处理：

我们获取到配置文件中设置的所有 `加密tag值` ，再找到文章中的 `tag标签` 。二者一对比，有匹配的项则说明该文章设置了 `tag值` 加密。

##### swig文件

要在 `.swig` 文件中实现相应的对比逻辑，就需要了解其使用的语法格式。而对于 `swig` 文件，使用的是 `Swig` 语法。

> `Swig` 是一个非常棒的、类似 `Django/jinja` 的 `node.js` 模板引擎。

不过看到这个代码库 [paularmstrong/swig: Take a swig of the best template engine for JavaScript.](https://github.com/paularmstrong/swig) 已经 `归档` 了。

但因为 `Swig` 是类似于 `jinja` 的模板引擎，那么我们直接去参考 `jinja` 的语法就可以了。

- [模板设计者文档 — Jinja2 2.7 documentation](http://docs.jinkan.org/docs/jinja2/templates.html)

##### 最终实现

获取全局配置中 `encrypt.tags` 的值：

```
{%- if (config.encrypt) and (config.encrypt.tags) %}
  {%- for ctag in config.encrypt.tags %}
    <span>{{ ctag.name }}</span>
  {%- endfor %}
{%- endif %}
```

在文章列表中获取当前文章包含的 `tags` 列表：

```
{%- if post.tags %}
  {%- for ptag in post.tags.toArray() %}
    <span>{{ ptag.name }}</span>
  {%- endfor %}
{%- endif %}
```

对于其中展示的文本格式，可以参考已有的 `发表于` `更新于` 这些副标题的格式来实现。

例如：

```
<span class="post-meta-item">
    <span class="post-meta-item-icon">
    <i class="far fa-calendar"></i>
    </span>
    <span class="post-meta-item-text">发表于</span>
    

    <time title="创建时间：2021-02-28 11:18:43 / 修改时间：11:41:19" itemprop="dateCreated datePublished" datetime="2021-02-28T11:18:43+08:00">2021-02-28</time>
</span>
```

对其进行优化，我们只需要显示提示文字，不需要后面的带下划线部分，最终得到的就是：

```
<span class="post-meta-item">
    <span class="post-meta-item-icon">
        <i class="fas fa-lock"></i>
    </span>
    <span class="post-meta-item-text">
        <font color='#FD7E13'>[加密]</font>
    </span>
</span>
```

整合上面的代码，对于文章中包含 `password` 的文档，通过如下方式来显示：

[![20210418170147](https://gitee.com/leafney/blogimage/raw/master/blog/20210418170147.png)](https://gitee.com/leafney/blogimage/raw/master/blog/20210418170147.png)

[20210418170147](https://gitee.com/leafney/blogimage/raw/master/blog/20210418170147.png)



相应代码：

```
{# 加密文章添加提示信息-for password #}
{%- if post.password %}
  <span class="post-meta-item">
      <span class="post-meta-item-icon">
          <i class="fas fa-lock"></i>
      </span>
      <span class="post-meta-item-text">
          <font color='#FD7E13'>[加密]</font>
      </span>
  </span>
{%- endif %}
```

对于文章中包含指定加密 `tags` 的文档，通过如下方式来显示：

[![20210418170209](https://gitee.com/leafney/blogimage/raw/master/blog/20210418170209.png)](https://gitee.com/leafney/blogimage/raw/master/blog/20210418170209.png)

[20210418170209](https://gitee.com/leafney/blogimage/raw/master/blog/20210418170209.png)



相应代码：

```
{# 加密文章添加提示信息-for config tags #}
    // 获取全局配置中的加密tag
    {%- if (config.encrypt) and (config.encrypt.tags) %}
      {%- for ctag in config.encrypt.tags %}
        // 判断当前文章中是否包含tags
        {%- if post.tags %}
          {%- for ptag in post.tags.toArray() %}
            // 如果有相同的tag值
            {%- if (ctag.name == ptag.name) %}
              // 显示加密提示信息
              <span class="post-meta-item">
                  <span class="post-meta-item-icon">
                      <i class="fas fa-lock"></i>
                  </span>
                  <span class="post-meta-item-text">
                      <font color='#FD7E13'>[加密]</font>
                  </span>
              </span>

            {%- endif %}

          {%- endfor %}
        {%- endif %}

      {%- endfor %}
    {%- endif %}
```

对于两种都有的文档，我们只需要通过一个 `判断` 来处理就好了：优先判断文档中的 `password` 字段。当文档中包含 `password` 时，就说明是加密文章；否则就去判断配置文件看是否为加密文章。

[![20210418170330](https://gitee.com/leafney/blogimage/raw/master/blog/20210418170330.png)](https://gitee.com/leafney/blogimage/raw/master/blog/20210418170330.png)

[20210418170330](https://gitee.com/leafney/blogimage/raw/master/blog/20210418170330.png)



最后的代码为：

```
{# 加密文章添加提示信息-for password #}
{%- if post.password %}
  <span class="post-meta-item">
      <span class="post-meta-item-icon">
          <i class="fas fa-lock"></i>
      </span>
      <span class="post-meta-item-text">
          <font color='#FD7E13'>[加密]</font>
      </span>
  </span>
{%- else %}
  {# 加密文章添加提示信息-for config tags #}
  {%- if (config.encrypt) and (config.encrypt.tags) %}
    {%- for ctag in config.encrypt.tags %}
      
      {%- if post.tags %}
        {%- for ptag in post.tags.toArray() %}
          {%- if (ctag.name == ptag.name) %}
            <span class="post-meta-item">
                <span class="post-meta-item-icon">
                    <i class="fas fa-lock"></i>
                </span>
                <span class="post-meta-item-text">
                    <font color='#FD7E13'>[加密]</font>
                </span>
            </span>
          {%- endif %}
        {%- endfor %}
      {%- endif %}

    {%- endfor %}
  {%- endif %}
{%- endif %}
```

稍微不好的一点就是，上面的操作是通过 `两个for循环` 来处理的，会导致一些性能问题。不过这个操作是在编译过程 `hexo g` 的时候来处理的，不影响博客浏览，也就可以忽略了。

------

#### 更换图标

对于需要显示的图标，可以从网站 [Icons | Font Awesome](https://fontawesome.com/icons) 中获取。

例如，我这里选择的是 `锁` 的icon图标，得到的代码如下：

```
<i class="fas fa-lock"></i>
```

------