<template>
	<div class="ebook">
		<div class="read-wrapper">
			<div id='read'></div>
			<div class="mask">
				<div class="left" @click="prevPage"></div>
				<div class="center"></div>
				<div class="right" @click="nextPage"></div>
			</div>
		</div>
	</div>
</template>

<script>
import Epub from 'epubjs'
const DOWNLOAD_URL = '/static/2018_Book.epub'	//载入下载电子书
global.ePub = Epub   // 设置全局的ePub对象

/*eslint-disable space-before-funtion-paren */

export default {
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
	},
	mounted() {
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