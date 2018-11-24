---
layout: post
title: "如何用octopress搭建个人静态博客"
date: 2018-11-23 14:13:45 +0800
comments: true
categories: octopress
---

> Octopress + Github 搭建个人博客

![Octopress](http://pi4lki9yr.bkt.clouddn.com/octopress.jpg)

<!-- more -->

- Octopress 是一个基于 Jekyll 博客引擎开发的博客框架，可以很方便的生成静态页面用于在 Github Pages 上展现。官方说法：

> A blogging framework for hackers。

本文主要介绍在 mac 下如何利用 Octopress 和 Github 搭建个人博客的基础环境。

#### 1. [安装Ruby](https://ruby-china.org/wiki/install_ruby_guide)

#### 2. [安装Git](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)

终端输入

``` 
    ssh-add ~/id_rsa
    ssh -T git@github.com 
```
如下结果

```
    Hi xxx!You've successfully authenticated, but GitHub does not provide shell access.
```
则表明git配置成功，接下来可以上传文件到 github 上了。

#### 3. 安装Octorpress

    git clone git://github.com/imathis/octopress.git octopress

修改安装源

    $ gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
    $ gem sources -l
    https://gems.ruby-china.com
    # 确保只有 gems.ruby-china.com

安装Octorpress相关插件
```
cd octopress
gem install bundler
bundle install
bundle exec rake install  # 安装 octopress 默认主题
bundle exec rake generate # 生成 octopress 博客
bundle exec rake preview  # 本地预览 octopress 博客

bundle exec rake 'new_post[title]'     # 新建博文文件
```

#### 4. 将 Octopress 博客部署到 github 上

登录github网站，创建一个仓库，必须命名为 username.github.io，username跟自己github用户名一致

    zgt0109.github.io
完整项目地址

    git@github.com:zgt0109/zgt0109.github.io.git

终端输入

    bundle exec rake setup_github_pages
    
会提示你输入github项目的Url路径，这个通过在 github 的项目右下方找到。例如
```
git@github.com:zgt0109/zgt0109.github.io.git
```

将项目部署到github上面
```
git add .   #注意add后面不要漏了 "."号
git commit -m "first commit"
git push origin source
bundle exec rake deploy # rake deploy 会直接 push Octopress目录中 master 分支
```

然后访问 

    zgt0109.github.io

不出意外的话，就能够看到自己的博客了。

#### 5. 优化 Octorpress

5.1替换Google JS公共库

Octopress 默认使用的是 Google 的 JS 公共库地址，加载的过程特别的慢。
所以我们要把它改为百度的 JS 公共库 ，需要把
```
/source/_includes/head.html
```
文件中的 Google 公共库地址

```js
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
```

改为：
```js
<script src="http://libs.baidu.com/jquery/1.9.1/jquery.min.js"></script>
```

5.2添加首页右侧 Category List

1.在 plugins 目录中创建category_list_tag.rb文件，文件内容如下：

```ruby
module Jekyll
  class CategoryListTag < Liquid::Tag
    def render(context)
      html = ""
      categories = context.registers[:site].categories.keys
      categories.sort.each do |category|
        posts_in_category = context.registers[:site].categories[category].size
        category_dir = context.registers[:site].config['category_dir']
        category_url = File.join(category_dir, category.gsub(/_|\P{Word}/, '-').gsub(/-{2,}/, '-').downcase)
        html << "<li class='category'><a href='/#{category_url}/'>#{category} (#{posts_in_category})</a></li>\n"
      end
      html
    end
  end
end

Liquid::Template.register_tag('category_list', Jekyll::CategoryListTag)
```

2.把下面内容添加到source/_includes/asides/category_list.html文件

```bash
<section> 
    <h1>Category List</h1>
    <ul id="categories"> 
        {% category_list %}  
    </ul>
</section>
```


3.修改_config.yml文件，用 command+f 找到default_asides，然后添加asides/category_list.html，值之间以逗号隔开。

```bash
default_asides: [asides/category_list.html, asides/recent_posts.html]
```


5.3添加社会化评论

1.登录 [Disqus](https://disqus.com/)，创建一个账号，然后点击 Add Disqus to your site，输入 Site name ，例如 boy，然后点击 Finish registration。这里页面会自动生成shortname，记录下来，待会有用。

2.打开_config.yml文件，找到

```
# Disqus Comments 
disqus_short_name:  
disqus_show_comment_count: false
```

设置 disqus_short_name 为刚刚创建的 Disqus 生成的 shortname，例如 zgt0109

把false改为true。


#### 6. 自定义网站域名
创建source/CNAME 文件并指定域名
```
$ echo 'zgt0109.xyz' > source/CNAME
```

重新生成博客并部署
```
bundle exec rake generate # 生成 octopress 博客
bundle exec rake deploy
```

首先申请一个域名，到阿里云、腾讯云上，例如

    zgt0109.xyz


将域名解析到github.io 上，主机记录值为192.30.252.153(154)

![如下图](http://pi4lki9yr.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-11-24%20%E4%B8%8A%E5%8D%8811.53.55.png)


等解析域名生效之后，访问 zgt0109.xyz 就可以看到我们的博客了