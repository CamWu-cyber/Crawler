# Facebook
困难一：对于Facebook的爬虫比较困难，因为Facebook官方为了保护用户的隐私，是不允许任何爬虫行为的。
困难二：只好采用selenium爬取，但是Facebook有些动态加载的页面，比如每条帖子的用户点赞列表，是需要进度条滚动加载的，不知道为什么网上找的所有滚动条加载的方法，对Facebook的弹窗界面都不管用。

解决：把www.facebook.com/balabala中的www改成mbasic.facebook.com/balabala. 据说mbasic打开的是original version of Facebook. 显示出的界面就是比较原始版本的样子，所有动态渲染的效果都没有了，用户列表也可以直接显示，不想要滚动加载。 直接使用xpath定位就能爬取数据啦~

PS: 如果mbasic不管用了，改用m.facebook.com/balabala
