# 基于`Hexo`的Fluid主题博客

## 依赖

1. `node.js`
2. `hexo`
3. `yarn`



## 插件

1. 生成目录插件：`"hexo-enhancer": "^1.0.10",`
2. 图片插件：`"hexo-asset-image": "github:CodeFalling/hexo-asset-image"`， 并设置 `post_asset_folder: true`。

 

## 配置

配置文件在根目录下的`_config.yml`中。



## 自定义命令

见`package.json`文件

```shell
"c": "git add . ; git commit -m \"auto\"",
"p": "yarn c ; git push",
"s": "hexo clean ; hexo generate ; hexo s",
"d": "hexo clean ; hexo generate ; hexo d",
"backup": "yarn c ; yarn p",
"cs": "yarn c ; yarn s",
"pd": "yarn p ; yarn d",
"total": "yarn c ; yarn pd"
```

如要在本地启动服务

```shell
yarn s
```

要部署到博客网站

```shell
yarn d
```



## 主题

要修改主题，只需将主题repo clone到`themes/`下，并修改根目录下的`_config.yml`文件的`theme`选项, 在根目录下添加主题对应的配置文件（一般在主题官网会告知配置方式）。

我现在用的是[fluid](https://github.com/fluid-dev/hexo-theme-fluid)主题, 目录为`themes/fluid/`

配置文件在根目录下，为`_config.fluid.xml`。



## 写博客

博客文章放在`source/_post/`文件夹下。

如果存在文件夹内，则会出现在**分类**页面下。否则仅仅出现在首页和归档下。

在分类页面下，文件夹是递归显示的。

假如我的目录是

```shell
source/_posts/
                ├── hello.md #不会显示在分类页面显示。
                ├── Articles #文件下省略文章显示
                ├── C++
                └── LeetCode
                    ├── Dynamic Programming
```

分类页面就是这样的，打开LeetCode还可以看到Dynamic Programming目录下的文章。

![image-20221120182302630](./README/image-20221120182302630.png)
