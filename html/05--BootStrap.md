# bootstrap #
	基于HTML ，CSS ，Javascript的前端框架，CSS/HTML框架
	
	* 响应式页面
		* 页面有能力去自动响应用户的设备环境
	* BootStrap的使用
		* 解压bootstrap-3.3.5-dist.zip，导入文件
		* 引入BootStrap的CSS和JS等
			* 引入核心css文件：
				<link rel="stylesheet" href=".../bootstrap.min.css"/>
			* 引入BootStrap主题文件（可选）:
				<link rel="stylesheet" href=".../bootstrap-theme.min.css"/>
			* 引入JQuery文件
				<script src=".../1.11.3/jquery.min.js"></script>
			* 引入核心javascript文件
				<script src=".../bootstrap.min.js"></script>
			* 为了确保适当的绘制和触屏缩放，需要在<head>中添加viewport元数据标签
				<meta name="viewport" content="width=device-width,initial-scale=1"/>
	* 全局CSS
		* BootStrap的框架提供的一系列的CSS样式，可以直接使用
	* 布局容器
		* BootStrap为页面内容和栅格系统包裹一个.container容器
			class="container"  / class="container-fluid"
	* 栅格系统
		* BootStrap提供了一套响应式、移动设备优先的流式栅格系统，随着屏幕或窗口尺寸的增加，系统会自动分为最多12列
		* 定义行：class="row"
		* 定义列：class="col-lg-n   / col-md-n  / col-sm-n / col-xs-n "(所有n的值相加需等于12)
			<div class="col-md-3 col-sm-6 col-xs-12"></div>
			<div class="col-md-3 col-sm-6 col-xs-12"></div>
			<div class="col-md-3 col-sm-6 col-xs-12"></div>
			<div class="col-md-3 col-sm-6 col-xs-12"></div>
	...