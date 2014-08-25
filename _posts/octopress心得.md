title: octopress心得
date: 2012-04-05 19:57
tags: octopress
---
<!-- more -->
###第一个问题，人人分享
addthis不支持人人

昨晚折腾了addthis的api，想法是去掉share,然后增加sinaweibo,然后自己加人人

因为人人的分享比较简单，参数的话template也可以配，是没什么难度的

但是吧，官方的开发文档是在是太坑了，已经post issue了

然后自己调了下人人自己的东西，但是样式方面，很难看，间距之类的，然后又不会改样式

瞬间苦逼了

###第二个问题, post url总是会在最后面加上奇奇怪怪的参数
一开始以为是主题的问题，重装了主题，发现好了

傻逼就在这个地方了，这个其实是addthis追踪分享的参数，我官方增加了追踪设定，加了一段js代码

![Address Bar Sharing](http://i1192.photobucket.com/albums/aa325/kongpo0412/ScreenShot2012-04-05at80717PM.png)

这个只是让别人复制你代码去xxx的时候带上追踪id

图好看，我现在就去掉了

###第三个问题，disqus是挂载进来的

之前以为shortname就是用户名，其实不然，这个是需要你自己去创建站点的

shortname自己设定，然后添加到你的_config.yml，具体的站点设定自己选择

有人可能会困惑与disqus旁边的那个计数器，我来说下吧，你的blog的留言是直接给你email的，计数器并不算在内的

disqus的提醒是分两种的

1. 计数器, 也就是notification类型的，当别人回复你的评论的时候，计数器会提醒的
2. email，你可以subscribe某篇文章，然后该文章的所有新留言都会直接email你的，还有就是当别人在你的blog留言的时候

disqus的优势很明显，wordpress的留言其实是很随意的，而且不人性化，每次要知道别人是否回复了，你必须勾选，选了之后还是email你的

disqus因为是挂载进来的，现在octopress的大军还是很强大的，最近找资料的blog都是octopress的，这无疑会方便很多，你也会更愿意去留言了

具体的设置呢，确实不好找，我也折腾半天才找到的，而且第一次找到之后又忘了，第二次去找不到了，翻了比较久的，在此方便大家去查询[传送门](http://disqus.com/account/notifications/)

###解决之路
分享的那个，我最后直接使用addthis的button_preferred样式了,然后贴上人人的js

首先推荐下人人的社交插件[传送门](http://dev.renren.com/website/?widget=plugin)

``` js
<script type="text/javascript" src="http://widget.renren.com/js/rrshare.js"></script>
<a name="xn_share" onclick="shareClick()" type="icon_large" href="javascript:;"></a>
<script type="text/javascript">
	function shareClick() {
		var rrShareParam = {
			resourceUrl : '',	//分享的资源Url
			pic : '',		//分享的主题图片Url
			title : '',		//分享的标题
			description : ''	//分享的详细描述
		};
		rrShareOnclick(rrShareParam);
	}
</script>
```
最后的样式就是这个样子了，人人我是不加统计的，主要是没需求

![分享+disqus](http://i1192.photobucket.com/albums/aa325/kongpo0412/ScreenShot2012-04-05at83243PM.png)
