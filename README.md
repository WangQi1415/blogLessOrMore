# LessOrMore


致谢
====================================
本项目拷贝自[https://github.com/luoyan35714/LessOrMore](https://github.com/luoyan35714/LessOrMore) ，感谢作者的无私奉献。  
由于多说关闭，导致原有的评论功能无法实现，因此采用gitment实现，项目地址[https://github.com/imsun/gitment](https://github.com/imsun/gitment) ，感谢孙士全大神的奉献。  
参考博客地址：[https://imsun.net/posts/gitment-introduction/#more](https://imsun.net/posts/gitment-introduction/#more) 
### gitment配置注意事项
需修改项目的_includes文件夹下的foot.html文件
```
	<div id="container"></div>
		<link rel="stylesheet" href="https://imsun.github.io/gitment/style/default.css">
		<script src="https://imsun.github.io/gitment/dist/gitment.browser.js"></script>
		<script>
		var gitment = new Gitment({
		  id: '页面 ID', // 可选。默认为 location.href
		  owner: 'WangQi1415',
		  repo: 'blogLessOrMore',
		  oauth: {
			client_id: '74e9a0f2d2811affe176',
			client_secret: 'bbb10cff4ba0ace9c8d49e7274cd3df39d23f664',
		  },
		})
		gitment.render('container')
		</script>
```
列处自己踩过的几个坑
1、owner需写成自己的用户名，比如本项目使用WangQi1415  
2、repo用来存储评论信息，需写成仓库的名称，如上，不需要添加任何前缀，最后是要能够通过https://github.com/owner/repo访问到仓库的位置  
3、配置oauth的时候callback路径应写为博客主页的URL，比如该项目为[www.wqblog.xyz](www.wqblog.xyz)   
若有转载请注明出处：[https://github.com/WangQi1415/blogLessOrMore](https://github.com/WangQi1415/blogLessOrMore) 

