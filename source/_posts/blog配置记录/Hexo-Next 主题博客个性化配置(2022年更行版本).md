---
title: Hexo-Next 主题博客个性化配置(2022年更新版本)
tags:
  - 博客
  - 记录
categories: 技术
abbrlink: '75999130'
date: 2022-03-01 09:10:00
---
# Hexo-Next 主题博客个性化配置(2022年更行版本)

## 网页预览：

[swimminghao.netlify.app](https://swimminghao.netlify.app/)
![在这里插入图片描述](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/01/rRBwpH.png)
因为本人比较喜欢简介风格的，所以整个界面都是`简约风格`的，一个好的博客，应该让人一眼就能看清楚技术分类,文章也应该就是文章，`让人能够最好的阅读你的博客` 这才是我们应该做的，所以没有太多花里胡哨的东西。

**使用工具：**

**Git
Github
visual studio code
Chrome**

## Hexo简易安装

### 前置条件

**软件版本**

**HEXO: 6.0.6
Hero-theme-next: 8.10.0**

### 安装hexo

```bash
npm install -g hexo-cli
```

# 主题下载安装

进入命令行，下载 [NexT](https://so.csdn.net/so/search?q=NexT&spm=1001.2101.3001.7020) 主题，输入：

```bash
git clone https://github.com/next-theme/hexo-theme-next/ themes/next
```

修改站点配置文件`_config.yml`，找到如下代码：

```bash
## Themes: https://hexo.io/themes/
theme: landscape => next
```

将 landscape 修改为 next 即可。

## 配置文件

在 Hexo 中有**两份**主要的配置文件，其名称都是 **_config.yml**。 其中，一份位于站点根目录下，主要包含 Hexo 本身的站点配置；另一份位于主**题目录下**，这份配置由主题作者提供，主要用于配置主题相关的选项。

为了描述方便，在以下说明中，将前者称为 **`站点配置文件`**， 后者称为 **`主题配置文件`**。

```bash
/hexo/_config.yml
/hexo/themes/next/_config.yml
```

## 修改语言

打开站点配置文件，搜索 language，找到如下代码：

```bash
author: authorName
language: zh-CN
timezone: Asia/Shanghai
```

## 新建标签及分类界面

打开 **主题配置文件**，搜索 menu，找到如下代码：

```bash
menu:
  home: / || fa fa-home
  about: /about/ || fa fa-user
  tags: /tags/ || fa fa-tags
  categories: /categories/ || fa fa-th
  archives: /archives/ || fa fa-archive
  #schedule: /schedule/ || fa fa-calendar
  sitemap: /sitemap.xml || fa fa-sitemap
  #commonweal: /404/ || fa fa-heartbeat
```

把 tags 和 categories 前面的 # 删除，

## 切换主题

next 主题自带**四种样式**。

在主题配置文件`/next/_config.yml`中查找：`scheme`，找到如下代码：

```bash
# Schemes
scheme: Muse
#scheme: Mist
#scheme: Pisces
#scheme: Gemini
选择你喜欢的一种样式，去掉前面的 #，其他主题前加上 # 即可。
```

## 隐藏网页底部 powered By Hexo / 强力驱动

打开 themes/next/layout/_partials/footer.njk

找到：

```java
{\%- if theme.footer.powered %}
  <div class="powered-by">
    {\%- set next_site = 'https://theme-next.js.org' if theme.scheme === 'Gemini' else 'https://theme-next.js.org/' + theme.scheme | lower + '/' %}
    {{- __('footer.powered', next_url('https://hexo.io', 'Hexo') + ' & ' + next_url(next_site, 'NexT.' + theme.scheme)) }}
  </div>
{\%- endif %}
```

把这段代码首尾分别加上：`<!--` 和`-->`，或者直接删除。

## 主页文章添加阴影

参考： [Hexo NexT 主题美化记录](https://www.jianshu.com/p/64caf29fdd0c)
打开`themes\next\source\css\_common\components\post\index.styl`文件，将`post-block`更改为如下代码：

```yml
if (hexo-config('motion.transition.post_block')) {
    .post-block{
        margin-top: 60px;
        margin-bottom: 60px;
        padding: 25px;
        background:rgba(255,255,255,0.9) none repeat scroll !important; //添加透明效果
        -webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
        -moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);
    }
    .pagination, .comments {
      opacity: 0;
    }
  }
```

## 页脚增加网站运行时间统计

1. 打开`themes/next/layout/_partials/footer.njk`文件，在如下图位置加入代码：

```js
{\%- if config.symbols_count_time.total_symbols or config.symbols_count_time.total_time %}
<div class="wordcount">
  {\%- if config.symbols_count_time.total_symbols %}
  <span class="post-meta-item">
    <span class="post-meta-item-icon">
      <i class="fa fa-chart-line"></i>
    </span>
    {\%- if theme.symbols_count_time.item_text_total %}
      <span>{{ __('symbols_count_time.count_total') + __('symbol.colon') }}</span>
    {\%- endif %}
    <span title="{{ __('symbols_count_time.count_total') }}">{{ symbolsCountTotal(site) }}</span>
  </span>
  {\%- endif %}

  {\%- if config.symbols_count_time.total_time %}
  <span class="post-meta-item">
    <span class="post-meta-item-icon">
      <i class="fa fa-coffee"></i>
    </span>
    {\%- if theme.symbols_count_time.item_text_total %}
      <span>{{ __('symbols_count_time.time_total') }} &asymp;</span>
    {\%- endif %}
    <span title="{{ __('symbols_count_time.time_total') }}">{{ symbolsTimeTotal(site, config.symbols_count_time.awl, config.symbols_count_time.wpm, __('symbols_count_time.time_minutes')) }}</span>
  </span>
  {\%- endif %}
</div>
{\%- endif %}

//此位置插入代码

{\%- if theme.busuanzi_count.enable %}
<div class="busuanzi-count">
```

倒计时代码：

```js
<span id="sitetime"></span>
<script language=javascript>
    function siteTime(){
        window.setTimeout("siteTime()", 1000);
        var seconds = 1000;
        var minutes = seconds * 60;
        var hours = minutes * 60;
        var days = hours * 24;
        var years = days * 365;
        var today = new Date();
        var todayYear = today.getFullYear();
        var todayMonth = today.getMonth()+1;
        var todayDate = today.getDate();
        var todayHour = today.getHours();
        var todayMinute = today.getMinutes();
        var todaySecond = today.getSeconds();
        /* Date.UTC() -- 返回date对象距世界标准时间(UTC)1970年1月1日午夜之间的毫秒数(时间戳)
        year - 作为date对象的年份，为4位年份值
        month - 0-11之间的整数，做为date对象的月份
        day - 1-31之间的整数，做为date对象的天数
        hours - 0(午夜24点)-23之间的整数，做为date对象的小时数
        minutes - 0-59之间的整数，做为date对象的分钟数
        seconds - 0-59之间的整数，做为date对象的秒数
        microseconds - 0-999之间的整数，做为date对象的毫秒数 */
        var t1 = Date.UTC(2022,01,04,00,00,00); //你的建站时间
        var t2 = Date.UTC(todayYear,todayMonth,todayDate,todayHour,todayMinute,todaySecond);
        var diff = t2-t1;
        var diffYears = Math.floor(diff/years);
        var diffDays = Math.floor((diff/days)-diffYears*365);
        var diffHours = Math.floor((diff-(diffYears*365+diffDays)*days)/hours);
        var diffMinutes = Math.floor((diff-(diffYears*365+diffDays)*days-diffHours*hours)/minutes);
        var diffSeconds = Math.floor((diff-(diffYears*365+diffDays)*days-diffHours*hours-diffMinutes*minutes)/seconds);
        document.getElementById("sitetime").innerHTML=" Run for "+diffYears+" Year "+diffDays+" Days "+diffHours+" Hours "+diffMinutes+" m "+diffSeconds+" s";
    }
    siteTime();
</script>
```

1. 在`themes\next\source\css\main.styl`文件中给倒计时添加样式

**不生效**

```css
#sitetime {
  background-image: -webkit-linear-gradient(left, #aa4b6b, #6b6b83, #3b8d99);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
```

## 浏览页面显示当前浏览进度

打开 themes/next/_config.yml，搜索关键字 `scrollpercent`，把 false 改为 true。

**效果图：**
![在这里插入图片描述](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/02/SIVZj0.png)

## Local Search本地搜索

安装插件`hexo-generator-searchdb`,执行以下命令:

```bash
npm install hexo-generator-searchdb --save	
```

修改`hexo/_config.yml`站点配置文件，新增以下内容到任意位置：

```yml
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

编辑 **主题配置文件**，启用本地搜索功能：

```yml
# Local search
local_search:
	enable: true
```

**效果图：**
![在这里插入图片描述](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/02/b1OAJr.png)

## 设置网站图标

在 [EasyIcon](https://www.easyicon.cc/) 中找一张（32 * 32）的 ico 图标，或者去别的网站下载或者制作，并将图标名称改为 favicon.ico，然后把图标放在 `/themes/next/source/images` 里，并且修改主题配置文件：

```yml
Put your favicon.ico into `hexo-site/source/` directory.
favicon: /favicon.ico
```

## 修改文章底部的#号的标签，改为图标

修改模板`/themes/next/layout/_macro/post.swig`

搜索 `rel="tag">{{ tag_indicate }}`，将 `{{ tag_indicate }}` 换成`<i class="fa fa-tag"></i>`

**效果图：**
![在这里插入图片描述](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/02/IPgd1C.png)

# 文章分享功能

打开`themes/next/_config.yml` 搜索关键字`needmoreshare2` 修改为下面设置

用npm卸载掉`hexo-next-share`，搜索所有`hexo-next-share`文件夹删除干净，然后切换到**网站文件夹**下，运行`npm install theme-next/hexo-next-share --save`,将以下代码都复制进**主题配置文件**里

```yml
# NeedMoreShare2
# Dependencies: https://github.com/theme-next/theme-next-needmoreshare2
# For more information: https://github.com/revir/need-more-share2
# iconStyle: default | box
# boxForm: horizontal | vertical
# position: top / middle / bottom + Left / Center / Right
# networks:
# Weibo | Wechat | Douban | QQZone | Twitter | Facebook | Linkedin | Mailto | Reddit | Delicious | StumbleUpon | Pinterest
# GooglePlus | Tumblr | GoogleBookmarks | Newsvine | Evernote | Friendfeed | Vkontakte | Odnoklassniki | Mailru
needmoreshare:
  enable: true
  cdn:
    js: //cdn.jsdelivr.net/gh/theme-next/theme-next-needmoreshare2@1/needsharebutton.min.js
    css: //cdn.jsdelivr.net/gh/theme-next/theme-next-needmoreshare2@1/needsharebutton.min.css
  postbottom:
    enable: true
    options:
      iconStyle: default
      boxForm: horizontal
      position: middleCenter
      networks: Weibo,Wechat,Douban,QQZone,Twitter,Facebook
  float:
    enable: false
    options:
      iconStyle: default
      boxForm: horizontal
      position: middleCenter
      networks: Weibo,Wechat,Douban,QQZone,Twitter,Facebook


# Likely Share
# See: https://ilyabirman.net/projects/likely/, https://github.com/ilyabirman/Likely
# Likely supports four looks, nine social networks, any button text.
# You are free to modify the text value and order of any network.
likely:
  enable: false
  cdn:
    js: //cdn.jsdelivr.net/npm/ilyabirman-likely@2/release/likely.min.js
    css: //cdn.jsdelivr.net/npm/ilyabirman-likely@2/release/likely.min.css
  look: light # available values: normal, light, small, big
  networks:
    twitter: Tweet
    facebook: Share
    linkedin: Link
    gplus: Plus
    vkontakte: Share
    odnoklassniki: Class
    telegram: Send
    whatsapp: Send
    pinterest: Pin

# share.js
# See: https://github.com/overtrue/share.js
# networks: weibo,qq,wechat,tencent,douban,qzone,linkedin,diandian,facebook,twitter,google
sharejs:
  enable: false
  cdn:
    js: //cdn.jsdelivr.net/npm/social-share.js@1/dist/js/social-share.min.js
    css: //cdn.jsdelivr.net/npm/social-share.js@1/dist/js/social-share.min.css
  networks: weibo,qq,wechat,tencent,douban,qzone,linkedin,diandian,facebook,twitter,google
  wechat_qrcode:
    title: share.title
    prompt: share.prompt
```

效果图：
`postbottom`为文章末尾分享 `float`则是在页面侧端分享
![在这里插入图片描述](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/02/ce3s7Z.png)

## 文章加密访问

参考链接： [hexo文章加密访问](https://swimminghao.netlify.app/posts/c30e6297/)

## 增加文章字数统计及阅读时常功能

安装字数统计插件 `npm i hexo-symbols-count-time`
在`hexo`的`_config.yml`下找到`# Extensions`在下面配置插件配置如下

```yml
# 字数统计插件 npm i hexo-symbols-count-time
symbols_count_time:
  symbols: true # 文章字数统计
  time: true # 文章时长统计
  total_symbols: true # 全局字数统计
  total_time: true # 全局时长统计
  exclude_codeblock: false  # 排除代码字数统计
```

## 文章置顶功能

移除默认安装的插件：

`npm uninstall hexo-generator-index --save`
安装新插件:

`npm install hexo-generator-index-pin-top --save`
最后编辑有这需求的相关文章时，在Front-matter（文件最上方以—分隔的区域）加上一行：

```
top: true
```

如果你置顶了多篇，怎么控制顺序呢？设置top的值（大的在前面），比如：

```java
# Post a.md
title: a
top: 1

# Post b.md
title: b
top: 10
1234567
```

文章 b 便会显示在文章 a 的前面

设置置顶图标
打开`/themes/next/layout/_macro/post.swig`文件，在`<div class="post-meta-container">`下方，插入如下代码：

```js
{\% if post.top %}
    <i class="fa fa-thumb-tack"></i>
    <font color=7D26CD>置顶</font>
    <span class="post-meta-divider">|</span>
{\% endif %}
```

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/02/FmHSJT.png)

## 修改[Read More]按钮样式

修改`themes/next/source/css/_common/components/post/index.styl`文件，加入自定义样式

```css
// [Read More]按钮样式
.post-button .btn {
    color: #555 !important;
    background-color: rgb(255, 255, 255);
    border-radius: 3px;
    font-size: 15px;
    box-shadow: inset 0px 0px 10px 0px rgba(0, 0, 0, 0.35);
    border: none !important;
    transition-property: unset;
    padding: 0px 15px;
}

.post-button .btn:hover {
    color: rgb(255, 255, 255) !important;
    border-radius: 3px;
    font-size: 15px;
    box-shadow: inset 0px 0px 10px 0px rgba(0, 0, 0, 0.35);
    background-image: linear-gradient(90deg, #a166ab 0%, #ef4e7b 25%, #f37055 50%, #ef4e7b 75%, #a166ab 100%);
}
```

**效果图：**
![在这里插入图片描述](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/03/ymZvXQ.png)

## 修改 阅读全文 前显示文字数量即位置

打开 `themes/next/_config.yml`，搜索关键字 `auto_excerpt`， 修改length即可修改阅读全文前显示文字数量

```yml
auto_excerpt:
  enable: true
  length: 150
```

**或者在文章中任意位置添加`<!-- more -->`**

建议在文章中加入 `<!-- more -->`
自定义 [Read More] 按钮之前要显示的内容！

## 修改链接文字样式

打开`themes/next/source/css/_common/components/post/index.styl`添加以下代码:

```yml
.post-body p a{
 color: #0593d3;
 border-bottom: none;
 &:hover {
   color: #ff106c;
   text-decoration: underline;
 }
}
```

**效果图：**
![在这里插入图片描述](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/03/6Ulm5b.png)

## 头像设置圆形,停留旋转效果

修改next主题配置文件,修改成以下代码:

```yml
# Sidebar Avatar
avatar:
  # Replace the default image and set the url here.
  url: /images/lion.png
  # lion.png放置在next/source/images文件夹下
  # If true, the avatar will be displayed in circle.
  rounded: true
  # If true, the avatar will be rotated with the cursor.
  rotated: false
```

**效果图：**
![在这里插入图片描述](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/03/lQxmpv.png)

## 增加近期文章

hexo主站`source` 目录下创建 `_data/sidebar.njk` 文件，加入如下内容：


```css
{# recent posts #}
{\% if theme.recent_posts %}
  <div class="links-of-blogroll motion-element {{ "links-of-blogroll-" + theme.recent_posts_layout  }}">
    <div class="links-of-blogroll-title">
      <!-- modify icon to fire by szw -->
      <i class="fa fa-history fa-{{ theme.recent_posts_icon | lower }}" aria-hidden="true"></i>
      {{ theme.recent_posts_title }}
    </div>
    <ul class="links-of-blogroll-list">
      {\% set posts = site.posts.sort('-date') %}
      {\% for post in posts.slice('0', '5') %}
        <li class="recent_posts_li">
          <a href="{{ url_for(post.path) }}" title="{{ post.title }}" target="_blank">{{ post.title }}</a>
        </li>
      {\% endfor %}
    </ul>
  </div>
{\% endif %}
```

并修改theme主题配置文件，取消sidebar的注释:
```yml
custom_file_path:
  #head: source/_data/head.njk
  #header: source/_data/header.njk
  sidebar: source/_data/sidebar.njk
  #postMeta: source/_data/post-meta.njk
  #postBodyEnd: source/_data/post-body-end.njk
  #footer: source/_data/footer.njk
  footer: source/_data/footer.swig
  #bodyEnd: source/_data/body-end.njk
  #variable: source/_data/variables.styl
  #mixin: source/_data/mixins.styl
  style: source/_data/styles.styl
```


编辑`themes/next/source/css/_common/outline/sidebar/sidebar-blogroll.styl`文件，标题溢出隐藏

```yml
.links-of-blogroll-list {
  list-style: none;
  margin: 0;
  padding: 0;
  text-align: cengter;
  display: block;
  word-break: keep-all;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

在 `themes/next/_config.yml`中修改成下方代码

```yml
# 近期文章设置
recent_posts_title: 近期文章
recent_posts_layout: block
recent_posts: true
```

**效果图：**
![在这里插入图片描述](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/03/heV4KR.png)

## 文章末尾添加"本文结束"标记

- 在目录`themes/next/layout/_macro/`下添加`passage-end-tag.swig`，内容如下： 
  ```html
  <div>
      {\% if not is_index %}
        <div style="text-align:center;color: #ccc;font-size:20px;">------------- 本 文 结 束&nbsp&nbsp&nbsp&nbsp&nbsp感 谢 您 的 阅 读 -------------</div>
      {\% endif %}
  </div>
  ```
  
- 打开`themes/next/layout/_macro/post.swig`文件，新增内容如下:
- ```HTML
  {\%- if theme.related_posts.enable and (theme.related_posts.display_in_home or not is_index) %}
     {{ partial('_partials/post/post-related.njk') }}
  {\%- endif %}
  //以下为新增代码
  <div>
      {\% if not is_index %}
      {\% include 'passage-end-tag.swig' %}
      {\% endif %}
   </div>
  ```
  
- 打开`主题配置文件`，添加代码如下：
- ```js
  # 文章末尾添加“本文结束”标记
  passage_end_tag:
  enabled: true
  ```

# 为博客加上妹子

live2d与busuanzi组件有bug冲突，安装了live2d，busuanzi就失效，所以我没有使用。

`npm install -save hexo-helper-live2d`
然后在在 hexo 的 _config.yml中添加参数：

```yml
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  log: false
  model:
    use: live2d-widget-model-<你喜欢的模型名字>s
  display:
    position: right
    width: 150
    height: 300
  mobile:
    show: true

12345678910111213141516
```

可供选择模型：

- `live2d-widget-model-chitose`
- `live2d-widget-model-epsilon2_1`
- `live2d-widget-model-gf`
- `live2d-widget-model-haru/01 (use npm install --save live2d-widget-model-haru)`
- `live2d-widget-model-haru/02 (use npm install --save live2d-widget-model-haru)`
- `live2d-widget-model-haruto`
- `live2d-widget-model-hibiki`
- `live2d-widget-model-hijiki`
- `live2d-widget-model-izumi`
- `live2d-widget-model-koharu`
- `live2d-widget-model-miku`
- `live2d-widget-model-ni-j`
- `live2d-widget-model-nico`
- `live2d-widget-model-nietzsche`
- `live2d-widget-model-nipsilon`
- `live2d-widget-model-nito`
- `live2d-widget-model-shizuku`
- `live2d-widget-model-tororo`
- `live2d-widget-model-tsumiki`
- `live2d-widget-model-unitychan`
- `live2d-widget-model-wanko`
- `live2d-widget-model-z16`

在站点目录下建文件夹`live2d_models`，

再在`live2d_models`下建文件夹`<你喜欢的模型名字`>,

再在`<你喜欢的模型名字>`下建json文件：`<你喜欢的模型名字>.model.json`

安装模型。在命令行（即Git Bash）运行以下命令即可：

```
npm install --save live2d-widget-model-<你喜欢的模型名字>
```

复制你喜欢的模型名字：

## 代码块复制选项

Next6 中自带了复制代码按钮，Next5 需要自己手动配置。

搜索 codeblock，找到如下配置：

```
codeblock:
border_radius: 8   # 按钮圆滑度
copy_button:  # 设置是否开启代码块复制按钮
	enable: true
	show_result: true  # 是否显示复制成功信息
```

## 修改加载特效

由于网页不可能一直都秒进，总会等待一段时间的，所以可以设置顶部加载条。Next 已经集成了很多加载特效，可以在下面选项中在线调试测试一下。

next主题配置文件搜索`pace`，找到如下代码：

```yml
# Progress bar in the top during page loading.
G# For more information: https://github.com/CodeByZach/pace
pace:
  enable: true
  # All available colors:
  # black | blue | green | orange | pink | purple | red | silver | white | yellow
  color: blue
  # All available themes:
  # big-counter | bounce | barber-shop | center-atom | center-circle | center-radar | center-simple
  # corner-indicator | fill-left | flat-top | flash | loading-bar | mac-osx | material | minimal
  theme: loading-bar
```

## 修改文章链接

在做次优化之前，hexo-next文章链接默认的生成规则是：`:year/:month/:day/:title`，是按照年、月、日、标题来生成的。
比如：`https://zxiaoxuan.github.io/2019/08/12/hello-world/` 这样，如果文章标题是中文的话，URL链接是也会是中文，
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190831093938325.png)

那么要生存简洁且唯一的URL，怎么办呢

**安装插件**

```js
npm install hexo-abbrlink --save
```

> 执行此命令可能会不成功，提示你缺少相应的依赖，比如babel-eslint、mini-css-extract-plugin、webpack-cli…
> 使用npm命令安装即可，比如npm install eslint@4.x babel-eslint@8 --save-dev

修改根目录站点配置文件`config.yml`，改为：

```yml
permalink: posts/:abbrlink/
abbrlink:
	alg: crc32   #算法： crc16(default) and crc32
	rep: hex     #进制： dec(default) and hex
```

生成的链接将会是这样的(官方样例)：
**四种可供选择**

```
crc16 & hex
https://post.zz173.com/posts/66c8.html

crc16 & dec
https://post.zz173.com/posts/65535.html
crc32 & hex
https://post.zz173.com/posts/8ddf18fb.html

crc32 & dec
https://post.zz173.com/posts/1690090958.html
12345678910
```

生成完后，原`md`文件的`Front-matter` 内会增加`abbrlink` 字段，值为生成的ID 。这个字段确保了在我们修改了Front-matter 内的博客标题title或创建日期date字段之后而不会改变链接地址。

## 评论 Waline 增强版

参考链接[Hexo NexT Waline评论]( [hexo文章加密访问](https://swimminghao.netlify.app/posts/a07389a8/))

## 各版块透明度修改

**内容板块透明**
博客根目录 `themes\next\source\css\_schemes\Pisces\_layout.styl`文件 `.content-wrap` 标签下 `background: white`修改为：

```
background: rgba(255,255,255,0.7); //0.7是透明度
```

**菜单栏背景**
博客根目录 `themes\next\source\css\_schemes\Pisces\_layout.styl`文件`.header-inner`标签下 background: white修改为：

```
background: rgba(255,255,255,0.7); //0.7是透明度
```

**站点概况背景**
博客根目录`themes\next\source\css\_schemes\Pisces\_sidebar.styl` 文件`.sidebar-inner` 标签下 background: white修改为：

```
background: rgba(255,255,255,0.7); //0.7是透明度
```

然后修改博客根目录`themes\next\source\css\_schemes\Pisces\_layout.styl`文件`.sidebar` 标签下 `background: $body-bg-color`修改为：

```
background: rgba(255,255,255,0.7); //0.7是透明度
```

**按钮背景**
博客根目录`themes\next\source\css\_common\components\post\post-button.styl` 同上修改对应位置为 `background: transparent;`

## 标签修改

打开`themes/next/layout/page.swig`

修改这里可以修改标签页的标签显示
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019092019070486.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FzNDgwMTMzOTM3,size_16,color_FFFFFF,t_70)

在这里添加东西会在标签页面上显示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190920190746932.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FzNDgwMTMzOTM3,size_16,color_FFFFFF,t_70)

## 彩色标签云

在`/themes/next/layout/`目录下，新增`tag-color.swig`文件，加入下方代码：

```js
<script type="text/javascript">
     var alltags = document.getElementsByClassName('tag-cloud-tags');
     var tags = alltags[0].getElementsByTagName('a');
     for (var i = tags.length - 1; i >= 0; i--) {
       var r=Math.floor(Math.random()*75+130);
       var g=Math.floor(Math.random()*75+100);
       var b=Math.floor(Math.random()*75+80);
       tags[i].style.background = "rgb("+r+","+g+","+b+")";
     }
</script>

<style>
  .tag-cloud-tags{
    /*font-family: Helvetica, Tahoma, Arial;*/
    /*font-weight: 100;*/
    text-align: center;
    counter-reset: tags;
  }
  .tag-cloud-tags a{
    border-radius: 6px;
    padding-right: 5px;
    padding-left: 5px;
    margin: 8px 5px 0px 0px;
  }
  .tag-cloud-tags a:before{
    content: "?";
  }

  .tag-cloud-tags a:hover{
     box-shadow: 0px 5px 15px 0px rgba(0,0,0,.4);
     transform: scale(1.1);
     /*box-shadow: 10px 10px 15px 2px rgba(0,0,0,.12), 0 0 6px 0 rgba(104, 104, 105, 0.1);*/
     transition-duration: 0.15s;
  }
</style>
```

在/themes/next/layout/page.swig/中引入tag-color.swig：

在下方加上 `{\% include 'tag-color.swig' %}` 代码

```js
 <div class="tag-cloud">
          <!--    <div class="tag-cloud-title">
                {{ _p('counter.tag_cloud', site.tags.length) }}
            </div>  -->
             <div class="tag-cloud-tags" id="tags">
			  {{ tagcloud({min_font: 16, max_font: 16, amount: 300, color: true, start_color: '#FFF', end_color: '#FFF'}) }}            
		  </div>
		</div>
+     {\% include 'tag-color.swig' %}
```

**或者将上方代码直接添加到下方**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190922224709732.png)

## 将标签云放到首页

在路径：`/themes/next/layout/index.swig` 中

`{\% block content %}`下面添加下方代码

```js
{\% block content %}

	<div class="tag-cloud">
	  <div class="tag-cloud-tags" id="tags">
		{{ tagcloud({min_font: 16, max_font: 16, amount: 300, color: true, start_color: '#fff', end_color: '#fff'}) }}
	  </div>
	</div>
	<br>
	
	{\% include 'tag-color.swig' %}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190920194842525.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FzNDgwMTMzOTM3,size_16,color_FFFFFF,t_70)

## 归档页美化

修改/themes/next/layout/_macro/post-collapse.swig后的代码如下：

```js
{\% macro render(post) %}

  <article class="post post-type-{{ post.type | default('normal') }}" itemscope itemtype="http://schema.org/Article">
    <header class="post-header">

      <{\% if theme.seo %}h3{\% else %}h2{\% endif %} class="post-title">
        {\% if post.link %}{# Link posts #}
          <a class="post-title-link post-title-link-external" target="_blank" href="{{ url_for(post.link) }}" itemprop="url">
            {{ post.title or post.link }}
            <i class="fa fa-external-link"></i>
          </a>
        {\% else %}
            <a class="post-title-link" href="{{ url_for(post.path) }}" itemprop="url">
              {\% if post.type === 'picture' %}
                {{ post.content }}
              {\% else %}
                <span itemprop="name">{{ post.title | default(__('post.untitled')) }}</span>
              {\% endif %}
            </a>
        {\% endif %}
      </{\% if theme.seo %}h3{\% else %}h2{\% endif %}>

      <div class="post-meta">
        <time class="post-time" itemprop="dateCreated"
              datetime="{{ moment(post.date).format() }}"
              content="{{ date(post.date, config.date_format) }}" >
          {{ date(post.date, 'MM-DD') }}
        </time>
      </div>

    </header>
  </article>

{\% endmacro %}
```
