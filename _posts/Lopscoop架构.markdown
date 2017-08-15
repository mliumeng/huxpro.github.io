# Lopscoop架构
Lopscoop 架构简述
## 业务详细整理
```
--|content 
	--|normal 
	--|video
		--|self
		--|other(fb,tw,ytb)
	--|quizzes
		--|ask
		--|pk
		--|vote
	--|...(more)
--|me
	--|login 
	--|register
	--|find
	--|points 
		--|task
		--|downline
		--|record
		--|withdrawal
	--|setting
	--|help

```
## 框架整理
<pre>
底层：ComponentVideoPlayer ComponentImageLoader netWork commonUtils commonConstant
业务：content
		  normal content
		  video
		  quizzes
	  	  ...
	  me
	  	  
		   login & register
		  points
		  setting
		   help
上层：APP view
</pre>

- 底层：公用组件（Component）、公用工具。
- 业务：各个业务（Model）。
- 上层：负责统一调配展示业务。

