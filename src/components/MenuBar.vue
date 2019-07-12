<template>
	<div class="menu-bar">
		<transition name="slide-up">		
			<div class="menu-wrapper" :class="{'hide-box-shadow':ifSettingShow || !ifTitleAndMenuShow}" v-show="ifTitleAndMenuShow">
				<div class="icon-wrapper">
					<span class="iconfont icon icon-menu">&#xe655;</span>
				</div>
				<div class="icon-wrapper">
					<span class="iconfont icon icon-progress">&#xe600;</span>
				</div>
				<div class="icon-wrapper">
					<span class="iconfont icon icon-bright" @click="showSetting(1)">&#xe614;</span>
				</div>
				<div class="icon-wrapper">
					<span class="iconfont icon icon-font" @click="showSetting(0)">&#xe64c;</span>
				</div>
			</div>
		</transition>
		<transition name="slide-up">
			<div class="setting-wrapper" v-show="ifSettingShow">
				<div class="setting-font-size" v-if="showTag === 0">
					<div class="preview" :style="{fontSize:fontSizeList[0].fontSize + 'px'}">A</div>
					<div class="select">
						<div class="select-wrapper" 
						v-for="(item,index) in fontSizeList" 
						:key="index"
						@click="setFontSize(item.fontSize)"
						>
							<div class="line"></div>
							<div class="point-wrapper">
								<div class="point" v-show="defaultFontSize===item.fontSize">
									<div class="small-point"></div>
								</div>
							</div>
							<div class="line"></div>
						</div>						
					</div>

					<div class="preview" :style="{fontSize:fontSizeList[fontSizeList.length - 1].fontSize + 'px'}">A</div>
				</div>
				<div class="setting-theme" v-else-if="showTag === 1">
					<div class="setting-theme-item" v-for="(item,index) in themeList" :key="index" @click="setTheme(index)">
						<div class="preview" :style="{background:item.style.body.background}" :class="{'no-border':item.style.body.background !== '#fff'}"></div>
						<div class="text" :class="{'selected':index === defaultTheme}">{{item.name}}</div>
					</div>
				</div>
			</div>
		</transition>
	</div>
</template>
<script>
export default {
	props:{
		ifTitleAndMenuShow:{
			type: Boolean,
			default: false,
		},
		fontSizeList: Array,
		defaultFontSize: Number,
		themeList: Array,
		defaultTheme: Number,
	},
	data() {
		return {
			ifSettingShow: false,
			showTag: 0,

		}
	},
	methods:{
		showSetting (tag) {
			this.ifSettingShow=true;
			this.showTag = tag;
		},
		hideSetting () {
			this.ifSettingShow = false;
		},
		setFontSize (fontSize) {
			this.$emit('setFontSize',fontSize);
		},
		setTheme (index) {
			this.$emit('setTheme',index);
		}
	}	
}
</script>
<style lang="scss" scoped>
@import '../assets/styles/global';

.menu-bar {
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
			&.hide-box-shadow {
				box-shadow: none;
			}
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
	.setting-wrapper {
		position:absolute;
		bottom: px2rem(48);
		left:0;
		z-index:101;
		width:100%;
		height: px2rem(60);
		background:white;
		box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, .15);
		.setting-font-size {
			display: flex;
			height:100%;
			.preview {
				flex: 0 0 px2rem(40);
				// background:red;
				@include center;
			}
			.select {
				display: flex;
				flex: 1;
			
				.select-wrapper {
					flex: 1;
					// background:yellow;
					display: flex;
					align-items:center;  //居中显示横线
					/*实际上被编译成:.select-wrapper:first-child .line:first-child*/
					/*意思是查找第一个select-wrapper类的第一个.line线*/
					&:first-child {
						.line {
							&:first-child {
								border-top:none;
							}
						}
					}
					&:last-child {
						.line {
							&:last-child {
								border-top:none;
							}
						}
					}
					.line {
						flex:1;
						height:0;
						border-top: px2rem(1) solid #ccc;
					}
					.point-wrapper {
						position: relative;
						flex: 0 0 0;
						width: 0;
						height: px2rem(7);
						border-left:px2rem(1) solid #ccc;
						.point {		/*外层圆球*/
							position: absolute;
							top:px2rem(-8);		//圆球位置
							left:px2rem(-10);
							width:px2rem(20);
							height:px2rem(20);
							border-radius:50%;
							background:white;
							border: px2rem(1) solid #ccc;
							box-shadow: 0 px2rem(4) px2rem(4) rgba(0,0,0,.15);
							@include center;
							.small-point {	/*内层圆球*/
								width:px2rem(5);
								height:px2rem(5);
								background:black;
								border-radius:50%;
							}
						}
					}
				}
			}
		}
		.setting-theme {
			height:100%;
			display:flex;
			.setting-theme-item {
				flex:1;
				display:flex;
				flex-direction:column;
				padding:px2rem(5);
				box-sizing:border-box;
				.preview {
					flex:1;
					border:px2rem(1) solid #ccc;
					box-sizing:border-box;
					&.no-border {
						border:none;
					}
				}
				.text {
					flex:0 0 px2rem(20);
					font-size:px2rem(14);
					color:#999;
					@include center;
					&.selected {
						color:#333;
					}
				}
			}
		}
	}
}

	
</style>