<template>
	<div class="ebook">
		<title-bar :ifTitleAndMenuShow="ifTitleAndMenuShow"></title-bar>
		<div class="read-wrapper">
			<div id='read'></div>
			<div class="mask">
				<div class="left" @click="prevPage"></div>
				<div class="center" @click="toggleTitleAndMenu"></div>
				<div class="right" @click="nextPage"></div>
			</div>
		</div>
		<menu-bar 
		:ifTitleAndMenuShow="ifTitleAndMenuShow"
		:fontSizeList="fontSizeList"
		:defaultFontSize="defaultFontSize"
		ref="menuBar"
		@setFontSize="setFontSize"
		></menu-bar>
	</div>
</template>

<script>
import TitleBar from '@/components/TitleBar'
import MenuBar from '@/components/MenuBar'

import Epub from 'epubjs'
const DOWNLOAD_URL = '/static/2019_Book.epub'	//载入下载电子书
global.ePub = Epub   // 设置全局的ePub对象

/*eslint-disable space-before-funtion-paren */


export default {
	components:{
		TitleBar,
		MenuBar,

	},
	data() {
		return {
			ifTitleAndMenuShow: false,
			fontSizeList: [
				{fontSize:12},
				{fontSize:14},
				{fontSize:16},
				{fontSize:18},
				{fontSize:20},
				{fontSize:22},
				{fontSize:24}
			],
			defaultFontSize:16
		}
	},
	methods:{
		//电子书的解析和渲染
		showEpub () {
			//解析电子书,生成Book对象
			this.book = new Epub(DOWNLOAD_URL);
			// console.log(this.book)
			
			//通过Book.renderTo生成Rendition对象(对象名不唯一)
			this.rendition = this.book.renderTo('read',{	//第一个参数是div的id
				width:window.innerWidth,
				height:window.innerHeight,
			})

			//通过Rendtion.display渲染电子书
			this.rendition.display();
			//获取Theme对象
			this.themes = this.rendition.themes;
			//设置默认字体
			this.setFontSize(this.defaultFontSize);
		},
		//翻页功能
		prevPage () {
			//rendition.prev方法实现上一页
			if(this.rendition){
				this.rendition.prev();
			}
		},
		nextPage () {
			//rendition.next方法实现下一页
				this.rendition.next();
		},
		//标题栏和菜单栏
		toggleTitleAndMenu () {
			//切换
			this.ifTitleAndMenuShow=!this.ifTitleAndMenuShow;
			if(!this.ifTitleAndMenuShow){
				//ref 加在子组件上,获取到的是组件实例,可以使用组件的所有方法,this.$refs.(ref值).组件方法,。
				this.$refs.menuBar.hideSetting();  
			}
		},
		//设置字体大小切换
		setFontSize (fontSize) {
			this.defaultFontSize = fontSize;
			if(this.themes){
				this.themes.fontSize(fontSize + 'px');
			}
		}
	},
	mounted() {
		//页面加载后
		this.showEpub()
	}
}
</script>

<style lang="scss" scoped>

$fontSize: 37.5;
@function px2rem($px){
	@return ($px / $fontSize) + rem;
}

@mixin center {
	display: flex;
	justify-content: center;
	align-items: center;
}

.ebook {	
	position: relative;

	.read-wrapper {
		.mask {
			position: absolute; 
			top: 0;
			left: 0;
			display: flex;
			width: 100%;
			height: 100%;
			.left {
				flex: 0 0 px2rem(100);
			}
			.center {
				flex: 1;
			}
			.right {
				flex: 0 0 px2rem(100);
			}
		}
	}

}

</style>