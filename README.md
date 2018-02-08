Name
====

lua-resty-htmlutils - 一个基于ngx-lua(openresty)的用于处理html标签相关的类库，可用于删除html标签，统计html中的文字长度，获取文章的摘要等

Menu
=================

* [Name](#name)
* [状态](#status)
* [描述](#description)
* [使用说明](#synopsis)
* [方法](#methods)
    * [truncate](#truncate)
    * [sub_html](#sub_html)
    * [strip_tags](#strip_tags)
    * [textlen](#textlen)
    * [strlen](#strlen)
* [TODO](#todo)
* [Author](#author)


status
======

该库目前处于测试状态

description
===========

基于openresty(ngx-lua)的html标签操作库，可用于截取文章摘要，删除html标签，统计html中文字字数，统计字符串的长度(包括中文)

synopsis
========

    
    #根据自己项目实际需要修改
    lua_package_path '/data/program/github/lua-resty-htmlutils/lib/?.lua;;';
    
    server {
        listen 8000;
       
        location /ueditor/upload {
            default_type application/json;
            content_by_lua_block {
                local html = require 'resty.htmlutils.htmlutils';
                local str = '<body><div class="goods">我们<i>真的不错</i></div><p>,另外一个</p></body>'
                --local newstr = html:strip_tags(str)
                local newstr, err = html:sub_html(str, 9)
            };
        }
        
        location /iconfont {
            default_type application/json;
            content_by_lua_block {
            　　 local iconfont = require 'resty.htmlutils.iconfont'
                -- local icon, err = iconfont:new({file = 'static/fonts/demo_fontclass.html'})
                local icon, err = iconfont:new({mode = 'http', url = '/static/fonts/demo_fontclass.html'})
                if not icon then
                    ngx.say('err:'..err)
                end
                local tab, err = icon:parse()
                if tab then
                   self.json(200, '', tab)
                else
                   self.json(500, 'failed')
                end
                -- tab structure
                --[[
                [
                    {
                        "class": "el-icon-blog-plugin",
                        "name": "接口"
                    }
                ]
                --]]
            };
        }
    }


[返回主菜单](#menu)

methods
=======

[返回主菜单](#menu)

truncate
---
`syntax: html = htmluntils:truncate(html, len)`

从html字符串中截取指定长度的字符(不包括标签部分)，一般用于获取博客的摘要等

参数如下:

* `html`

    要操作的html字符串内容(如博客内容)
    
* `len`

    要截取的长度   

[返回主菜单](#menu)

sub_html
-------
`syntax: html = html:sub_html(html, len)`

truncate方法的别名

strip_tags
---
`syntax: text = htmluntils:strip_tags(html)`

删除html内容中的所有标签

参数如下:

* `html`

    要操作的html字符串内容(如博客内容)
 
textlen
---
`syntax: len = htmluntils:textlen(html)`

获取html内容(不包括标签，中文一个字符算一个长度)的长度

参数如下:

* `html`

    要计算的html字符串内容(如博客内容)
      
strlen
---
`syntax: text = htmluntils:strlen(str)`

计算字符串的长度(可以计算中文，一个中文算一个长度)

参数如下:

* `str`

    要计算的字符串长度    
    
iconfont:new
---
`syntax: tab, err = iconfont:new(options)`
    
iconfont初始化设置

参数如下：

* options : 是一个table,表示初始化选项配置

    * mode : 模式(file表示本地文件;http表示本地网络请求)
    * file : 本地文件路径
    * root : 本地文件基本路径
    * url  : 本地网络请求地址
    
用法如下:
    
    `local iconfont = require 'resty.htmlutils.iconfont'
     local icon, err = iconfont:new({file = 'static/fonts/demo_fontclass.html'})
     `    
    
    
iconfont:parse
---
`syntax: tab, err = iconfont:parse()`
    
将iconfont的演示文件解析为一个table
    
用法如下:
    
    `local iconfont = require 'resty.htmlutils.iconfont'
     -- local icon, err = iconfont:new({file = 'static/fonts/demo_fontclass.html'})
     local icon, err = iconfont:new({mode = 'http', url = '/static/fonts/demo_fontclass.html'})
     tab, err = icon:parse()`

[返回主菜单](#menu)

Author
======

shixinke (诗心客) <ishixinke@qq.com>  www.shixinke.com


[返回主菜单](#menu)

