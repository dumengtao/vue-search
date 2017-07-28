<template>
	<!-- 选择logo组件 -->
	<div class="main-logo">
		<img :src="logoData[selectedNow].imgSrc" @click="toggleFlag">
		<span class="logoList-arrow" @click="toggleFlag"></span>
		<!-- tranition是vue的过渡效果 -->
		<transition name="logofade">
			<ul v-show="logoListFlag&&mouseLeaveFlag" class="logoList" @mouseleave="mouseLeaveFlag">
				<li v-for="(item,index) in logoData" class="logoItem" @mouseover="logoListHover(index)" :class="{ selectback:index==logoNow }" @click="logoSelected(index)">
					<img :src="item.imgSrc">
				</li>
			</ul>
		</transition>	
	</div>
</template>

<script>
	// export defult是es6中的方法，将整个花括号对象暴露出去;
	export default {
		data: function() {
			return {
				selectedNow: 0,
				logoNow: -1,
				logoListFlag: false,
				mouseLeaveFlag: false,
				logoData: [
					{ imgSrc: require('../assets/360_logo.png') },
					{ imgSrc: require('../assets/baidu_logo.png') },
					{ imgSrc: require('../assets/sougou_logo.png') }
				]
			}
		},
		methods: {
			toggleFlag: function(){
				this.logoListFlag = !this.logoListFlag;
				this.mouseLeaveFlag = true;
			},
			logoListHover: function(index){
				this.logoNow = index;
			},
			logoSelected: function(index){
				this.selectedNow = index;
				this.logoListFlag = false;
				// 触发父组件的自定义事件，向父组件传参数,selectNow是选择了哪个搜索引擎的索引，父组件得到了之后就可以指定搜索时跳转到哪个搜索引擎
				this.$emit('getindex',this.selectedNow);
			},
			mouseLeaveFlag: function(){
				//鼠标离开列表事件
				this.mouseLeaveFlag = false;
				this.logoListFlag = false;
			}
		}
	}
</script>

<style>
	ul{
		list-style: none;
		padding: 0;
		margin: 0;
	}

	.main-logo{
		width: 600px;
		height: 140px;
		position: relative;
	}

	.main-logo img{
		display: block;
		margin: 0 auto;
		user-select: none; /*文本不能被选择*/ 
		cursor: pointer;
	}

	.logoList-arrow{
		position: absolute;
		width: 0;
		height: 0;
		border: 8px solid;
		border-color: #000 transparent transparent transparent; /*transparent边框颜色为透明。*/
		right: 100px;
		top: 66px;
		cursor: pointer;
	}

	.logoList{
		width: 200px;
		position: absolute;
		top: 100%;
		left: 50%;
		margin-left: -100px;
		z-index: 999999;
		border: 1px solid #d4d4d4;
	}

	.logoList li{
		width: 100%;
		height: 80px;
		background-color: #fefefe;
		line-height: 80px;
		padding-top: 1px;
	}

	.logoList li img{
		width: 100%;
		margin-top: 10px; 
		/*ul的宽是200px，图片原来款480px；
		当图片宽变为200px时，高为58.33px左右，li内高边距为1px，总体来说60px左右；
		li高为80px，使图片上下居中，则margin-top为10px*/
	}

	.logofade-enter-active,
	.logofade-leave-active{
		transition: all .5s;
	}

	.logofade-enter,
	.logofade-leave-active{
		opacity: 0;
		transform: translateY(20px);
	}

	.selectback{
		background-color: #eee !important;
		cursor: pointer;
	}
</style>
