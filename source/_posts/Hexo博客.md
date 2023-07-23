---
title: Hexo博客
date: 2021/10/24 10:24:00
updated: 2023/07/20 10:24:00
tags: Hexo博客
categories: Hexo博客
---
Hexo+GitHub搭建个人博客以及使用Next主题进行美化。

<!-- more -->

## 前言

[Hexo](https://hexo.io/zh-cn/)是一个快速、简洁且高效的博客框架。Hexo 使用Markdown（或其他标记语言）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。本文讲述Hexo+GitHub搭建个人博客以及使用Next主题进行美化。

本文默认你已经安装好Git和Node.js工具。本文所述中的站点配置文件为Hexo根目录下的`_config.yml`，主题配置文件为`\themes\next`下的`_config.yml`

## 安装Hexo及部署到GitHub

### 安装Hexo

1. 使用 npm 安装 Hexo。

    ``` bash
    npm install -g hexo-cli
    ```

2. 安装 Hexo 完成后，执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

    ```bash
    hexo init <folder>
    cd <folder>
    npm install
    ```

    新建完成后，指定文件夹的目录如下：

    ```bash
    .
    ├── _config.yml     # 网站配置信息
    ├── package.json    # 应用程序的信息
    ├── scaffolds       # 模版文件夹
    ├── source          # 资源文件夹
    |   ├── _drafts
    |   └── _posts
    └── themes          # 主题 文件夹
    ```

3. 执行如下命令，之后启动浏览器，访问网址：http://localhost:4000/ 即可本地预览

    ``` bash
    hexo clean
    hexo generate
    hexo server
    ```

### 部署Hexo到Github

1. 建立名为 `<你的 GitHub 用户名>.github.io` 的代码仓库。

2. 安装`hexo-deployer-git`

    ``` bash
    npm install hexo-deployer-git --save
    ```

3. 编辑站点配置文件`_config.yml` 中添加以下配置：

    ```yaml
    deploy:
      type: git
      repo: https://github.com/<username>/<username>.github.io
      # example, https://github.com/hexojs/hexojs.github.io
      branch: master
    ```

4. 执行以下命令部署到GitHub：

    ``` bash
    hexo clean
    hexo generate
    hexo deploy
    ```

5. 浏览 `<GitHub 用户名>.github.io` 检查你的网站能否运作。

### Hexo绑定域名(可选)

1. 获取域名，域名购买网址:[https://cn.aliyun.com](https://cn.aliyun.com/) (阿里云官网)

2. 域名解析，打开阿里云域名控制台，在域名列表，选择域名解析，添加三条解析记录。

    ```
    第一条:记录类型:A，主机记录:@，记录值:192.30.252.154
    
    第二条:记录类型:A，主机记录:@，记录值:192.30.252.153
    
    第三条:记录类型:CNAME，主机记录:www，记录值:<GitHub 用户名>.github.io
    ```

3. 进入Hexo根目录，在source目录下新建记事本文件，输入域名，保存命名为`CNAME`，格式为所有文件(无.txt后缀)

4. 执行以下命令部署到GitHub：

    ``` bash
    hexo clean
    hexo generate
    hexo deploy
    ```
    
5. 打开Hexo代码仓库，在Settings->Pages的`Custom domain`中输入域名，点击save。

## Hexo使用NexT主题美化

### 安装NexT主题

NexT主题的官网地址：https://theme-next.iissnan.com/

1. 在Hexo根目录下，执行如下命令：

    ```bash
    git clone https://github.com/iissnan/hexo-theme-next themes/next
    ```

2. 编辑站点配置文件， 找到 `theme`字段，并将其值更改为 next。

    ```yaml
    theme: next
    ```

3. 主题设定

    Scheme 是 NexT 提供的一种特性，借助于 Scheme，NexT 为你提供多种不同的外观。同时，几乎所有的配置都可以 在 Scheme 之间共用。目前 NexT 支持四种 Scheme，他们是：

    - Muse
    - Mist
    - Pisces
    - Gemini
    
    Scheme 的切换通过更改主题配置文件，将你启用的 `scheme` 前面注释 `#` 去除即可。
    
    ```yaml
    # Schemes
    #scheme: Muse
    #scheme: Mist
    #scheme: Pisces
    scheme: Gemini
    ```

### 配置菜单

1. 编辑主题配置文件，将你启用的菜单项前面注释 `#` 去除即可。

    ```yaml
    menu:
      home: / || fa fa-home
      #about: /about/ || fa fa-user
      tags: /tags/ || fa fa-tags
      categories: /categories/ || fa fa-th
      archives: /archives/ || fa fa-archive
      #schedule: /schedule/ || fa fa-calendar
      #sitemap: /sitemap.xml || fa fa-sitemap
      #commonweal: /404/ || fa fa-heartbeat
    ```

2. 生成标签页和分类页。

    ```bash
    hexo new page tags
    hexo new page categories
    ```

3. 在source目录编辑刚新建的页面，将页面的类型设置为`tags`和`categories`，主题将自动为这个页面显示标签云。页面内容如下：

    ```markdown
    title: 标签
    date: 2022-10-24 10:24:00
    type: “tags"
    ```

    ```markdown
    title: 分类
    date: 2022-10-24 10:24:00
    type: “categories"
    ```

4. 修改`tags`标签图标，编辑主题配置文件，修改如下内容：

    ```yaml
    # Use icon instead of the symbol # to indicate the tag at the bottom of the post
    tag_icon: true
    ```

### 配置侧边栏

#### 站点基本信息

编辑站点配置文件，修改如下内容：

```yaml
# Site
title: xxx  #网站标题
subtitle: xxx  #网站副标题
description: xxx  #个性签名
keywords:
author: xxx  #作者
language: zh-CN  #站点语言
timezone: ''  #时区
```

#### 社交信息

Next 默认给出了一些模板，将你启用的社交平台前面注释`#`号去除并修改其中的链接即可。

```yaml
social:
  GitHub: https://github.com/<username> || fab fa-github
  E-Mail: mailto:xxx@gmail.com || fa fa-envelope
  #Weibo: https://weibo.com/yourname || fab fa-weibo
  #Google: https://plus.google.com/yourname || fab fa-google
  #Twitter: https://twitter.com/yourname || fab fa-twitter
  #FB Page: https://www.facebook.com/yourname || fab fa-facebook
  #StackOverflow: https://stackoverflow.com/yourname || fab fa-stack-overflow
  #YouTube: https://youtube.com/yourname || fab fa-youtube
  #Instagram: https://instagram.com/yourname || fab fa-instagram
  #Skype: skype:yourname?call|chat || fab fa-skype
```

#### 头像设置

1. 选取头像文件，存放在`\themes\next\source\images`中。

2. 编辑主题配置文件。

    ```yaml
    avatar:
      # Replace the default image and set the url here.
      url: /images/author.jpg
      # If true, the avatar will be dispalyed in circle.
      rounded: true
      # If true, the avatar will be rotated with the cursor.
      rotated: true
    ```

### 配置本地搜索

1. 安装`hexo-generator-search`

    ```
    npm install hexo-generator-searchdb --save
    ```

2. 编辑站点配置文件

    ```yaml
    search:
      path: search.xml
      field: post
      format: html
      limit: 10000
    ```

3. 编辑主题配置文件

    ```yaml
    local_search:
      enable: true
      # If auto, trigger search by changing input.
      # If manual, trigger search by pressing enter key or search button.
      trigger: auto
      # Show top n results per article, show all results by setting to -1
      top_n_per_article: 1
      # Unescape html strings to the readable one.
      unescape: false
      # Preload the search data when the page loads.
      preload: false
    ```

### 页面配置

#### 顶部加载条

编辑主题配置文件，找到`pace`改为true，并从提供的样式中选择相应样式即可。

```yaml
pace:
  enable: true
  # Themes list:
  # big-counter | bounce | barber-shop | center-atom | center-circle | center-radar | center-simple
  # corner-indicator | fill-left | flat-top | flash | loading-bar | mac-osx | material | minimal
  theme: minimal
```

#### 浏览进度条

编辑主题设置文件，修改如下内容：

```yaml
back2top:
  enable: true
  # Back to top in sidebar.
  sidebar: false
  # Scroll percent label in b2t button.
  scrollpercent: true
```

#### 鼠标点击小红心

1. 在`themes/next/source/js/`下新建`clicklove.js`，内容如下：

    ```javascript
    !function (e, t, a) { function n() { c(".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: fixed;}.heart:after{top: -5px;}.heart:before{left: -5px;}"), o(), r() } function r() { for (var e = 0; e < d.length; e++)d[e].alpha <= 0 ? (t.body.removeChild(d[e].el), d.splice(e, 1)) : (d[e].y--, d[e].scale += .004, d[e].alpha -= .013, d[e].el.style.cssText = "left:" + d[e].x + "px;top:" + d[e].y + "px;opacity:" + d[e].alpha + ";transform:scale(" + d[e].scale + "," + d[e].scale + ") rotate(45deg);background:" + d[e].color + ";z-index:99999"); requestAnimationFrame(r) } function o() { var t = "function" == typeof e.onclick && e.onclick; e.onclick = function (e) { t && t(), i(e) } } function i(e) { var a = t.createElement("div"); a.className = "heart", d.push({ el: a, x: e.clientX - 5, y: e.clientY - 5, scale: 1, alpha: 1, color: s() }), t.body.appendChild(a) } function c(e) { var a = t.createElement("style"); a.type = "text/css"; try { a.appendChild(t.createTextNode(e)) } catch (t) { a.styleSheet.cssText = e } t.getElementsByTagName("head")[0].appendChild(a) } function s() { return "rgb(" + ~~(255 * Math.random()) + "," + ~~(255 * Math.random()) + "," + ~~(255 * Math.random()) + ")" } var d = []; e.requestAnimationFrame = function () { return e.requestAnimationFrame || e.webkitRequestAnimationFrame || e.mozRequestAnimationFrame || e.oRequestAnimationFrame || e.msRequestAnimationFrame || function (e) { setTimeout(e, 1e3 / 60) } }(), n() }(window, document);
    ```

2. 编辑`themes/next/layout/_layout.swig` 文件，添加如下内容：

    ```swig
    {% if theme.clicklove %}
    	<script type="text/javascript" src="/js/clicklove.js"></script>
    {% endif %}
    ```

3. 编辑主题配置文件，添加如下内容：

    ```yaml
    clicklove: true
    ```

#### 底部布局

编辑主题配置文件，根据注释自行修改如下内容：

```yaml
footer:
  # Specify the date when the site was setup. If not defined, current year will be used.
  since: 2020

  # Icon between year and copyright info.
  icon:
    # Icon name in Font Awesome. See: https://fontawesome.com/icons
    name: fa fa-heart
    # If you want to animate the icon, set it to true.
    animated: true
    # Change the color of icon, using Hex Code.
    color: "#ff0000"

  # If not defined, `author` from Hexo `_config.yml` will be used.
  copyright: 昔年

  # Powered by Hexo & NexT
  powered: false

  # Beian ICP and gongan information for Chinese users. See: https://beian.miit.gov.cn, http://www.beian.gov.cn
  beian:
    enable: false
    icp:
    # The digit in the num of gongan beian.
    gongan_id:
    # The full num of gongan beian.
    gongan_num:
    # The icon for gongan beian. See: http://www.beian.gov.cn/portal/download
    gongan_icon_url:
```

### 文章内容相关设置

#### 文章内容部分显示

1. 编辑主题配置文件，修改内容如下：

    ```yaml
    # Read more button
    # If true, the read more button would be displayed in excerpt section.
    read_more_btn: true
    ```
    
2. 手动形成摘要，在文章想要截取内容之后添加`<!--more-->`

#### 文章字数和阅读时间统计

1. 安装`hexo-symbols-count-time`

    ```bash
    npm install hexo-symbols-count-time --save
    ```

2. 编辑站点配置文件，添加如下内容：

    ```yaml
    symbols_count_time:
      symbols: true 
      time: true
      total_symbols: true
      total_time: true
    ```

3. 编辑主题配置文件，修改内容如下：

    ```yaml
    # Post wordcount display settings
    # Dependencies: https://github.com/theme-next/hexo-symbols-count-time
    symbols_count_time:
      separated_meta: true
      item_text_post: true
      item_text_total: true
      awl: 4
      wpm: 25
    ```

#### 代码块

编辑主题配置文件，按照注释自行修改内容。

```yaml
codeblock:
  # Code Highlight theme
  # Available values: normal | night | night eighties | night blue | night bright | solarized | solarized dark | galactic
  # See: https://github.com/chriskempson/tomorrow-theme
  highlight_theme: night
  # Add copy button on codeblock
  copy_button:
    enable: true
    # Show text copy result.
    show_result: true
    # Available values: default | flat | mac
    style: mac
```

行内代码和超链样式

在 `themes\next\source\css\main.styl` 文件，添加如下内容：

```stylus
code {
  padding: 2px 4px;
  word-wrap: break-word;
  color: #c7254e;
  background: #f9f2f4;
  border-radius: 3px;
  font-size: 18px;
}

.post-body p a,
.post-body li a {
  color: #0593d3;
  border-bottom: none;
  border-bottom: 1px solid #0593d3;
  &:hover {
    color: #fc6423;
    border-bottom: none;
    border-bottom: 1px solid #fc6423;
  }
}
```

#### 文章结束标记

1. 在`\themes\next\layout\_macro` 中新建 `post-end-tag.swig` 文件，并添加以下内容：

```swig
<div>
  {% if not is_index %}
    <div style="text-align:center;color:#bfbfbf;font-size:16px;">
      <span>-------- 本文结束 </span>
      <i class="fa fa-{{ config.post_end_tag.icon }}"></i>
      <span> 感谢阅读 --------</span>
    </div>
  {% endif %}
</div>
```

2. 编辑`\themes\next\layout_macro\post.swig`文件，添加如下内容：

```swig
{% if config.post_end_tag.enabled and not is_index %}
	<div>
		{% include 'post-end-tag.swig' %}
	</div>
{% endif %}
```

3. 编辑主题配置文件，添加如下内容：

    ```yaml
    post_end_tag:
      enabled: true
      icon: paw
    ```

#### 添加版权信息

1. 编辑站点配置文件，修改如下内容：

    ```yaml
    # URL
    ## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
    url: http://www.example.com
    permalink: :year/:month/:day/:title/
    permalink_defaults:
    pretty_urls:
      trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
      trailing_html: true # Set to false to remove trailing '.html' from permalinks
    ```

2. 编辑主题配置文件，修改如下内容：

    ```yaml
    # Creative Commons 4.0 International License.
    # See: https://creativecommons.org/share-your-work/licensing-types-examples
    # Available values of license: by | by-nc | by-nc-nd | by-nc-sa | by-nd | by-sa | zero
    # You can set a language value if you prefer a translated version of CC license, e.g. deed.zh
    # CC licenses are available in 39 languages, you can find the specific and correct abbreviation you need on https://creativecommons.org
    creative_commons:
      license: by-nc-sa
      sidebar: false
      post: true
      language:
    ```

### 看板娘

1. 安装`hexo-helper-live2d`

```bash
npm install --save hexo-helper-live2d
```

2. 安装具体模型形象

```bash
npm install <packagename>
```

3. 编辑站点配置文件，添加如下内容：

```yaml
live2d:
  enable: true
  pluginModelPath: assets/
  model:
    use: <packagename>  
  display:
    position: left
    width: 150 
    height: 300
  mobile:
    show: false   
  rect:
    opacity:0.7
```

## Hexo备份

Hexo部署到Github的是Hexo编译后的文件，这些文件是用来生成网页的，并不包含我们的源文件。接下来讲述如何备份保存Hexo源文件。

### Hexo备份

1. 在Hexo部署的GitHub代码仓库新建代码分支，命名为backup，并设置为默认分支。

2. 将刚刚创建的分支clone到本地，我们只需要其中的`.git`文件。把clone下来的项目中的`.git`文件复制到Hexo根目录下。如果搭建博客时clone过主题文件的，请把主题文件里面的`.git`文件删除。

3. 在Hexo根目录下，执行如下命令：

    ```bash
    git add .
    git commit -m "xxx"
    git push origin hexo
    ```

4. 之后每次更新博客，执行如下命令：

```bash
hexo clean
git add .
git commit -m "xxx"
git push
hexo generate
hexo deploy
```

### Hexo恢复

1. 将backup分支clone到本地，进入项目，执行如下命令：

    ```bash
    npm install hexo-cli
    npm install
    npm install hexo-deployer-git
    ```

2. 将之前安装过的其他插件安装即可。
