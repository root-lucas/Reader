<template>
	<div class="ebook">
	<transition name="slide-down">
		<div class="title-wrapper" v-show="ifTitleAndMenuShow">
			<div class="left">
				<span class="iconfont icon">&#xe617;</span>
			</div>
			<div class="right">
				<div class="icon-wrapper icon">
					<span class="iconfont">&#xe61d;</span>
				</div>
				<div class="icon-wrapper">
					<span class="iconfont icon">&#xe63a;</span>
				</div>
				<div class="icon-wrapper">
					<span class="iconfont icon">&#xe65b;</span>
				</div>
			</div>
		</div>
	</transition>
		<div class="read-wrapper">
			<div id='read'></div>
			<div class="mask">
				<div class="left" @click="prevPage"></div>
				<div class="center" @click="toggleTitleAndMenu"></div>
				<div class="right" @click="nextPage"></div>
			</div>
		</div>
	<transition name="slide-up">
		<div class="menu-wrapper" v-show="ifTitleAndMenuShow">
			<div class="icon-wrapper">
				<span class="iconfont icon icon-menu">&#xe655;</span>
			</div>
			<div class="icon-wrapper">
				<span class="iconfont icon icon-progress">&#xe600;</span>
			</div>
			<div class="icon-wrapper">
				<span class="iconfont icon icon-bright">&#xe614;</span>
			</div>
			<div class="icon-wrapper">
				<span class="iconfont icon icon-font">&#xe64c;</span>
			</div>
		</div>
	</transition>
	</div>
</template>

<script>
import Epub from 'epubjs'
const DOWNLOAD_URL = '/static/2019_Book.epub'	//载入下载电子书
global.ePub = Epub   // 设置全局的ePub对象

/*eslint-disable space-before-funtion-paren */

export default {
	data() {
		return {
			ifTitleAndMenuShow: false,
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
			this.rendition.display()
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
		},
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
	.title-wrapper {
			position:absolute;
			top:0;
			left:0;
			z-index:101;
			width:100%;
			display:flex;
			height:px2rem(48);
			background:white;
			box-shadow: 0 px2rem(8) px2rem(8) rgba(0,0,0,.15);
		.left {
			flex: 0 0 px2rem(60);
			@include center;
		}	
		.right {
			flex: 1;
			display:flex;
			justify-content:flex-end;
			.icon-wrapper {
				flex: 0 0 px2rem(40);
				@include center;
				.icon-cart {
					font-size:px2rem(22);
				}
			}
		}
	}
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
	.menu-wrapper {
			position:absolute;
			bottom:0;
			left:0;
			z-index:101;
			display:flex;
			width:100%;
			height:px2rem(48);
			background:white;
			box-shadow: 0 px2rem(-8) px2rem(8) rgba(0,0,0,.15);
			.icon-wrapper {
				flex:1;
				@include center;
				.icon-progress {
					font-size:px2rem(28)
				}
				.icon-bright {
					font-size:px2rem(24)
				}
			}	
	}
}

</style>