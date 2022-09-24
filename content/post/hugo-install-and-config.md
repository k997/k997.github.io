---
title: Hugo
date: 2022-09-17T04:27:53.000Z
draft: false
---

# hugo 安装及配置

1.  github 创建仓库
2.  git clone 仓库至本地
3.  `hugo new site . --force -f yml`
    `--force`表示在已存在文件夹下创建网站
    `-f yml`表示 config 使用 YAML 格式
4.  ```bash
    git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
    git submodule update --init --recursive
    ```
5.  编辑配置文件
    ```yaml
    baseURL: "https://k997.github.io"
    title: kong
    paginate: 7
    theme: PaperMod

    enableInlineShortcodes: true
    enableRobotsTXT: true
    buildDrafts: false
    buildFuture: false
    buildExpired: false
    enableEmoji: true
    pygmentsUseClasses: true
    # googleAnalytics: UA-123-45

    minify:
        disableXML: true
        minifyOutput: true

    menu:
    main:
        - identifier: archives
        name: 归档
        url: archives/
        weight: 5
        - identifier: search
        name: 搜索
        url: search/
        weight: 20
        - identifier: categories
        name: 分类
        url: categories/
        weight: 10
        - identifier: tags
        name: 标签
        url: tags/
        weight: 10

    outputs:
        home:
            - HTML
            - RSS
            - JSON

    params:
        env: production # to enable google analytics, opengraph, twitter-cards and schema.
        description: "kong's blog - k997.github.io"
        author: kong
        # author: ["Me", "You"] # multiple authors

        defaultTheme: auto
        # disableThemeToggle: true

        ShowShareButtons: true
        ShowReadingTime: true
        ShowPostNavLinks: true
        ShowBreadCrumbs: true
        ShowCodeCopyButtons: true
        ShowToc: true

        disableSpecial1stPost: false
        disableScrollToTop: false

        displayFullLangName: true
        # comments: false
        hidemeta: false
        hideSummary: false

        images: ["papermod-cover.png"]

        profileMode:
            enabled: false
            title: "kong's blog"
            imageUrl: "#"
            # imageTitle: my image
            # imageWidth: 120
            # imageHeight: 120
            buttons:
                - name: Archives
                url: archives
                - name: Tags
                url: tags

        homeInfoParams:
            Title: "kong's blog"
            Content: >
                一个喜欢折腾的人的简单博客
            # - 短横线后的内容跟上一行的内容在网页上被识别为一行
        socialIcons:
            - name: github
            url: "https://github.com/k997"
    #        - name: KoFi
    #          url: "https://ko-fi.com/adityatelange"
            - name: RsS
            url: "index.xml"
    # jump github to make suggestions 
        editPost:
            URL: "https://github.com/k997/k997.github.io/tree/main/content"
            Text: "你对文章的建议" # edit text
            appendFilePath: true # to append file path to Edit link

        twikoo:
            # 版本需和twikoo的版本号要对得上
            version: "1.6.7" 
            envId: "https://twikoo.kongwg.top"
        # label:
        #     text: "Home"
        #     icon: icon.png
        #     iconHeight: 35

        # analytics:
        #     google:
        #         SiteVerificationTag: "XYZabc"

        # assets:
        #     favicon: "<link / abs url>"
        #     favicon16x16: "<link / abs url>"
        #     favicon32x32: "<link / abs url>"
        #     apple_touch_icon: "<link / abs url>"
        #     safari_pinned_tab: "<link / abs url>"

        # cover:
        #     hidden: true # hide everywhere but not in structured data 是否在下面两种情况下显示
        #     hiddenInList: true # hide on list pages and home 是否在列表视图中显示
        #     hiddenInSingle: true # hide on single page 是否在单页视图中显示

        # fuseOpts:
        #     isCaseSensitive: false
        #     shouldSort: true
        #     location: 0
        #     distance: 1000
        #     threshold: 0.4
        #     minMatchCharLength: 0
        #     keys: ["title", "permalink", "summary", "content"]



    taxonomies:
        category: categories
        tag: tags
        series: series

    markup:
        goldmark:
            renderer:
                unsafe: true
    #     highlight:
    #         # anchorLineNos: true
    #         codeFences: true
    #         guessSyntax: true
    #         lineNos: true
    #         # noClasses: false
    #         style: monokai

    privacy:
        vimeo:
            disabled: false
            simple: true

        twitter:
            disabled: false
            enableDNT: true
            simple: true

        instagram:
            disabled: false
            simple: true

        youtube:
            disabled: false
            privacyEnhanced: true

    services:
        instagram:
            disableInlineCSS: true
        twitter:
            disableInlineCSS: true

    ```
## 汉化
1. 分类

    content 目录下新建 `categories\_index.md`:
    ```yaml
    ---
    title: "分类"
    ---
    ```
2. 标签
    content 目录下新建 `tags\_index.md`:
    ```yaml
    title: "标签"
    ```
3. 归档
    content 目录下新建 `archives.md`:
    ```yaml
    ---
    title: "归档"
    layout: "archives"
    # url: "/archives"
    summary: "archives"
    ---
    ```
4. 搜索
    content 目录下新建 `search.md`:
    ```yaml
    ---
    title: "搜索"
    layout: "search"
    ---
    ```