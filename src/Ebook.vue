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
		@setFontSize="setFontSize"
		:themeList="themeList"
		:defaultTheme="defaultTheme"
		@setTheme="setTheme"
		:bookAvailable="bookAvailable"
		@onProgressChange="onProgressChange"
		:navigation="navigation"
		@jumpTo="jumpTo"
		ref="menuBar"
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
			defaultFontSize:16,
			themeList: [
				{
					name:'default',
					style:{
						body:{
							'color':'#000',background:'#fff'
						}
					}
				},
				{
					name:'eye',
					style:{
						body:{
							'color':'#000',background:'#ceeaba'
						}
					}
				},
				{
					name:'night',
					style:{
						body:{
							'color':'#fff',background:'#000'
						}
					}
				},
				{
					name:'gold',
					style:{
						body:{
							'color':'#000',background:'rgb(241,236,226)'
						}
					}
				},
			],
			defaultTheme:0,
			bookAvailable: false,
			navigation: null,
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
			/*thems的俩个方法:
				主题注册:this.themes.register(name,styles)
				主题切换:this.themes.select(name)
			*/
			this.registerTheme();
			this.setTheme(this.defaultTheme);
			//获取Locations对象
			//通过epubjs钩子函数来实现电子书定位
			this.book.ready.then(() => {
				//生成目录
				this.navigation = this.book.navigation;
				// console.log(this.navigation);
				return this.book.locations.generate()
			}).then(result => {
				this.locations = this.book.locations;
				// this.onProgressChange(50); //实现跳转到百分之50的位置
				this.bookAvailable = true;
			})
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
		},
		registerTheme () {
			this.themeList.forEach(theme => {
			  //注册全部主题颜色样式
		      this.themes.register(theme.name, theme.style)
		    })
		},
		setTheme (index) {
			//切换主题
			this.themes.select(this.themeList[index].name);
			this.defaultTheme = index;
		},
		//propress进度条的数值(0-100)
		onProgressChange (progress) {
			const percentage = progress / 100;
			const location = percentage > 0 ? this.locations.cfiFromPercentage(percentage):0;
			this.rendition.display(location)
		},
			hideTitleAndMenu () {
			//隐藏标题栏和菜单栏
			this.ifTitleAndMenuShow = false;
			//隐藏菜单栏弹出的设置栏
			this.$refs.menuBar.hideSetting();
			//隐藏目录
			this.$refs.menuBar.hideContent();
		},
		//根据链接跳转到指定位置(目录)
		jumpTo (href) {
			this.rendition.display(href);
			this.hideTitleAndMenu();
		},
	},
	mounted () {
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