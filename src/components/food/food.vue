<template>
	<transition name="move">
		<div v-show="showFlag" class="food" ref="food">
			<div class="image-wrapper">
				<img :src="food.image">
				<div class="back">
					<i class="icon-arrow_lift" @click="hide()"></i>
				</div>
			</div>
			<div class="content">
				<h1 class="title">{{food.name}}</h1>
				<div class="detail">
					<span class="sell-count">月售{{food.sellCount}}份</span>
					<span class="rating">好评率{{food.rating}}</span>
				</div>
				<div class="price">
					<span class="now">￥{{food.price}}</span>
					<span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
				</div>
			</div>
			<div class="cartcontrol-wrapper">
				<cartcontrol :food="food"></cartcontrol>
			</div>
			<div class="buy"></div>
		</div>
	</transition>
</template>

<script type="text/ecmascript-6">
	import BScroll from 'better-scroll';
	import cartcontrol from 'components/cartcontrol/cartcontrol';

	export default {
		props: {
			food: Object
		},
		data () {
			return {
				showFlag: false
			};
		},
		methods: {
			show () {
				this.showFlag = true;
				this.$nextTick(() => {
					if (!this.scroll) {
						this.scroll = new BScroll(this.$refs.food, {
							click: true
						});
					}
					this.scroll.refresh();
				});
			},
			hide () {
				this.showFlag = false;
			}
		},
		components: {
			cartcontrol
		}
	};
</script>

<style lang="stylus" rel="stylesheet/stylus">
.food
	position fixed
	left 0px
	top 0px
	bottom 48px
	z-index 30
	width 100%
	background #fff
	transform: translate3d(0, 0, 0)
	&.move-enter-active, &.move-leave-active
		transition: all 0.2s linear
	&.move-enter, &.move-leave-active
		transform: translate3d(100%, 0, 0)
	.image-wrapper
		position relative
		width 100%
		height 0
		padding-top 100%
		background red
		img
			position absolute
			top 0
			left 0
			width 100%
			height 100%
		.back
			position absolute
			top 10px
			left 0px
			.icon-arrow_lift
				display block
				padding 10px
				color #fff
				font-size 20px
	.content
		position relative
		padding 18px
		.title
			line-height 14px
			margin-bottom 8px
			font-size 14px
			font-weight 700
			color rgb(7,17,27)
		.detail
			margin-bottom 18px
			line-height 10px
			height 10px
			font-size 0
			.rating, .sell-count
				font-size 10px
				color rgb(147,153,159)
			.sell-count
				margin-right 12px
		.price
			font-weight 700
			line-height 24px
			.now
				margin-right 8px
				font-size 14px
				color rgb(240,20,20)
			.old
				text-decoration line-through
				font-size 10px
				color rgb(147,153,159)
</style>
