title: wordpress主题开发教程     
date: 2014-01-05 08:06:17  
tags:   
    - PHP  
    - wordpress     
    - 二次开发     
 
---

WordPress是一种使用PHP语言开发的博客平台，免费、开源。

作为最流行的博客平台，海量的插件和主题可以说是WordPress 最大的特色了，没有哪一个Blog 平台拥有如此多的插件和主题，拥有如此多的用户和爱好者。  

![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/6597301763611602432.png)  

下面介绍如何开发设计你自己的 WordPress 主题。  

### 主题的剖析  
WordPress主题目录位于 wp-content/themes/。主题目录拥有所有样式文件、模板文件、可选的函数文件 (functions.php)、JavaScript 文件、图片等。比如说一个叫做 "test" 的主题就会放在 wp-content/themes/test/目录里。  

WordPress 主题一般由三种文件构成：  
1. 样式表文件 style.css， 控制着页面的外观。  
2. 函数文件 (functions.php)。  
3. 模板文件，它控制着从数据库中调出的数据所呈现的外观。

### 主题样式表 

style.css为该主题的主题样式，该样式文件须在文件开头以注释的形式列出主题的详细信息。  

```css  
/*

Theme Name: ykqTheme
Theme URI: yuankeqiang.lofter.com
Author: ykq
Tag : hehe
.......

*/  

```  
这样启用主题后wordpress会自动读取该主题的相关信息，我们可以在后台->外观->主题->主题详情 来查看我们我们的主题信息。  

![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/1828742923789379496.jpg)  

缩略图来自哪里：

在主题目录创建一张图片重命名为screenshot.png或者screenshot.jpg，wordpress会自动读取该图片为主题的缩略图。

还有一个样式rtl.css：

作用是如果网站的阅读方向是自右向左的，他会自动被包含进来。你可以使用 the RTLer 插件来生成这个文件。

### 函数文件  
一个主题可以使用一个函数文件，位于主题的根目录，叫做 functions.php。

这个文件就像一个插件， 如果它位于你正在使用的主题里的话，他在你的主题初始化的时候就会自动加载(后台页面和前台页面都一样加载)。对于这个文件的建议： 

* 启用主题功能，例如：侧边栏，菜单，文章缩略图，文章格式，自定义标题栏。
* 定义用于模板文件中的函数。
* 设置一个选项菜单，让网站拥有者可以自定义颜色，样式，和你的主题的其他特性。


### 模板文件  
模板是一些PHP文件，不同的模板代表不同的页面：

常见模板如下：

* index.php 主模板.必须.
* comments.php 留言区域文件(包括留言列表和留言框) 
* single.php 日志单独页面模板。显示单独的一篇文章时被调用。  
* single-{post-type}.php 自定义单独页面模板。例如， 
* single-books.php 展示自定义文章类型为 books的文章。
* page.php 页面文件 
* archvie.php 分类和日期存档页文件
* tag.php 标签模板。
* author.php 作者模板。作者页面调用
* search.php 搜索页面文件  
* header.php 网页头部文件 
* sidebar.php 网页侧边栏文件 
* footer.php 网页底部文件 
* image.php 图片附件模板。当在wordpress中查看单个图片时将调用此模板
* 404.php 错误页面 模板。当WordPress无法查找到匹配查询的日志或页面时，使用404.php文件。


### 自定义单页模板  
我们可以自定义页面模板page.php，创建一个自定义页面需要首先创建一个文件，建议文件的命名为page-{name}.php,

假设我们创建一个公司简介的页面，自定义页面可以叫做page-about.php。在page-about.php的文件顶部必须写上页面名称：

```php  
<?php /* Template Name: 公司简介  */  ?>  
```  
这样我们创建页面的时候就可以选择我们自定义的模板：  
![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/6599333661098147095.jpg)  

注意：我们可以在主题文件夹的任何地方创建页面模板，若模板文件很多的话我们可以创建一个文件夹如page-templates，然后将模板文件都放到该文件夹下。  

### 包含模板  

为了加载其他模板(除了 header, sidebar, footer 这些已经被预先定义了加载命令的例如 get_header())到某个模板中，你可以使用 get_template_part()。这利于主题的代码重用。  

### Wordpress 函数代码

创建完模板，如何在模板中动态显示内容？那就要用到wordpress的函数代码了  

WordPress 中定义了许多有用的 PHP 函数,大概有几百个。参考：[http://codex.wordpress.org/zh-cn:%E5%87%BD%E6%95%B0%E5%8F%82%E8%80%83](http://codex.wordpress.org/zh-cn:%E5%87%BD%E6%95%B0%E5%8F%82%E8%80%83)  

一些常见的函数有：  

```php 

<?php the_content(); ?> 日志内容 
<?php if(have_posts()) : ?> 确认是否有日志 
<?php while(have_posts()) : the_post(); ?> 如果有，则显示全部日志 
<?php endwhile; ?> 结束PHP函数”while” 
<?php endif; ?> 结束PHP函数”if” 
<?php get_header(); ?> header.php文件的内容 
<?php get_sidebar(); ?> sidebar.php文件的内容 
<?php get_footer(); ?> footer.php文件的内容 
<?php the_time(’m-d-y’) ?> 显示格式为”10-12-13″的日期 
<?php comments_popup_link(); ?> 显示一篇日志的留言链接 
<?php the_title(); ?> 显示一篇日志或页面的标题 
<?php the_permalink() ?> 显示一篇日志或页面的永久链接/URL地址 
<?php the_category(’, ‘) ?> 显示一篇日志或页面的所属分类 
<?php the_author(); ?> 显示一篇日志或页面的作者 
<?php the_ID(); ?> 显示一篇日志或页面的ID 
<?php edit_post_link(); ?> 显示一篇日志或页面的编辑链接 
<?php get_links_list(); ?> 显示链接 
<?php comments_template(); ?> comments.php文件的内容 
<?php wp_list_pages(); ?> 显示一份博客的页面列表 
<?php wp_list_cats(); ?> 显示一份博客的分类列表 
<?php next_post_link(’ %link ‘) ?> 下一篇日志的URL地址 
<?php previous_post_link(’%link’) ?> 上一篇日志的URL地址 
<?php get_calendar(); ?> 调用日历 
<?php wp_get_archives() ?> 显示一份博客的日期存档列表 
<?php posts_nav_link(); ?> 显示较新日志链接(上一页)和较旧日志链接（下一页） 
<?php bloginfo(’description’); ?> 显示博客的描述信息

<?php bloginfo(’name’); ?> 网站标题 
<?php wp_title(); ?> 日志或页面标题 
<?php bloginfo(’stylesheet_url’); ?> Wordpress主题样式表文件style.css的相对地址 
<?php bloginfo(’pingback_url’); ?> Wordpress博客的Pingback地址 
<?php bloginfo(’template_url’); ?> Wordpress主题文件的相对地址 
<?php bloginfo(’version’); ?> 博客的Wordpress版本 
<?php bloginfo(’url’); ?> Wordpress博客的绝对地址 
<?php bloginfo(’charset’); ?> 网站的字符编码格式  

```  

### 其他  

到目前为止基本讲完了wordpress的主题开发

简单的说，开发一个wordpress分以下几步：

1. 在wp-content/themes/文件夹下创建属于你自己的主题文件夹
2. 然后在主题文件夹下创建2个必须的基本文件 index.php 和style.css 
3. 接着根据需要创建其他模板和文件及利用wordpress内置的函数在模板中动态显示内容
4. 最后登陆后台在外观中启用我们创建的主题就可以了

当然，还有个简便的方法就是把wordpress默认的主题复制一份,然后在其基础上二次开发就OK了。


### 最后  

什么？看完了文章你还不会wordpress的主题开发？

那一定是你看的方式不对，请刷个牙再来看一遍。