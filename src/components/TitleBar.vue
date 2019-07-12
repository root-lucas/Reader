<template>
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
</template>
<script>
export default {
	props:{
		ifTitleAndMenuShow:{
			type:Boolean,
			default:false,
		}
	}
}
</script>
<style lang="scss" scoped>
@import '../assets/styles/global';
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
</style>