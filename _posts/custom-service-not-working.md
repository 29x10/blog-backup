title: custom service not working
date: 2012-04-05 08:41
tags: addthis
---
<!-- more -->
i have read this
in order to have renren.com working
[custom-service](http://support.addthis.com/customer/portal/articles/381245-custom-services)

i convert something like this
``` html
	<script type="text/javascript">
	  var addthis_config = {
	  services_custom: {
	  name: "Renren",
	  url: "http://widget.renren.com/dialog/share?resourceurl={{url}}&title={{title}}",
	  icon: "http://dev.renren.com/views/website/inc/images/share06.png"}
	  }
	</script>

	<a href="http://widget.renren.com/dialog/share"
	   class="addthis_button_widget.renren.com"></a>
```

but it didn't work for me. even the example provided.

here is what i got from the example(use the `example.share.com` example)
``` html
<a class="addthis_button_share.example.com at300b" href="http://www.addthis.com/bookmark.php?v=250&amp;winname=addthis&amp;pub=ra-4f794b38399d380c&amp;source=tbx-250&amp;lng=en-US&amp;s=share.example.com&amp;url=http%3A%2F%2F127.0.0.1%3A4000%2Fmarkdown%2F2012%2F04%2F04%2Foctopressda-jian-xiang-jie-macosx-and-and-fedora%2F&amp;title=octopress%E6%90%AD%E5%BB%BA%E8%AF%A6%E8%A7%A3(MacOSX%26%26Fedora)%20-%20Blazing%20Five&amp;ate=AT-ra-4f794b38399d380c/-/-/4f7c7ad8d05f6200/2&amp;frommenu=1&amp;uid=4f7c7ad89041b1ae&amp;ufbl=1&amp;ct=1&amp;rsi=4f7c7a2bbf2d8ca3&amp;gen=4&amp;pre=http%3A%2F%2F127.0.0.1%3A4000%2F&amp;tt=0" target="_blank" title="Send to Share.example.com"><span class="at300bs at15nc at15t_share.example.com"></span></a>
```
