<template>
	<div class="shopcart">
		<div class="content">
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo" :class="{'highlight':totalCount>0}">
						<i class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></i>
					</div>
					<div class="num" v-show="totalCount>0">{{totalCount}}</div>
				</div>
				<div class="price" :class="{'highlight':totalPrice>0}">{{totalPrice}}</div>
				<div class="desc">另需配送费{{deliveryPrice}}元</div>
			</div>
			<div class="content-right">
				<div class="pay" :class="{'enough':totalPrice>=minPrice}">
					{{payDesc}}
				</div>
			</div>
		</div>
		<div class="ball-container">
			<div v-for="(ball,index) in balls" :key="index">
				<transition name="drop" @before-enter="beforeDrop" @enter="dropping" @after-enter="afterDrop">
					<div class="ball" v-show="ball.show">
						<div class="inner inner-hook"></div>
					</div>
				</transition>
			</div>
		</div>
	</div>
</template>

<script type="text/ecmascript-6">
	export default {
		props: {
			selectedFoods: {
				type: Array,
				default () {
					return [{
						price: 10,
						count: 1
					}, {
						price: 13,
						count: 3
					}];
				}
			},
			deliveryPrice: {
				type: Number,
				default: 0
			},
			minPrice: {
				type: Number,
				default: 0
			}
		},
		data () {
			return {
				balls: [
				{
					show: false
				},
				{
					show: false
				},
				{
					show: false
				},
				{
					show: false
				},
				{
					show: false
				}],
				dropBalls: []
			};
		},
		methods: {
			drop (el) {
				for (let i = 0; i < this.balls.length; i++) {
					let ball = this.balls[i];
					if (!ball.show) {
						ball.show = true;
						ball.el = el;
						this.dropBalls.push(ball);
						return;
					}
				}
			},
			beforeDrop (el) {
				let count = this.balls.length;
				while (count--) {
					let ball = this.balls[count];
					if (ball.show) {
						let rect = ball.el.getBoundingClientRect();
						let x = rect.left - 32;
						let y = -(window.innerHeight - rect.top - 52);
						el.style.display = '';
						el.style.webkitTransform = `translate3d(0,${y}px,0)`;
						el.style.transform = `translate3d(0,${y}px,0)`;
						let inner = el.getElementsByClassName('inner-hook')[0];
						inner.style.webkitTransform = `translate3d(${x}px,0,0)`;
						inner.style.transform = `translate3d(${x}px,0,0)`;
						console.log('beforeDrop' + el.style);
					}
				}
			},
			dropping (el, done) {
				/* eslint-disable no-unused-vars */
				let rf = el.offsetHeight;
				this.$nextTick(() => {
					el.style.webkitTransform = 'translate3d(0,0,0)';
					el.style.transform = 'translate3d(0,0,0)';
					let inner = el.getElementsByClassName('inner-hook')[0];
					inner.style.webkitTransform = 'translate3d(0,0,0)';
					inner.style.transform = 'translate3d(0,0,0)';
					el.addEventListener('transitionend', done);
				});
			},
			afterDrop (el) {
				let ball = this.dropBalls.shift();
				if (ball) {
					ball.show = false;
					el.style.display = 'none';
				}
			}
		},
		computed: {
			totalPrice () {
				let total = 0;
				this.selectedFoods.forEach((food) => {
					total += food.price * food.count;
				});
				return total;
			},
			totalCount () {
				let total = 0;
				this.selectedFoods.forEach((food) => {
					total += food.count;
				});
				return total;
			},
			payDesc () {
				if (this.totalPrice === 0) {
					return 	`￥${this.minPrice}元起送`;
				} else if (this.totalPrice < this.minPrice) {
					return `还差￥${this.minPrice - this.totalPrice}元起送`;
				} else {
					return '去结算';
				}
			}
		}
	};
</script>

<style lang="stylus" rel="stylesheet/stylus">
.shopcart
	width 100%
	height 48px
	position fixed
	left 0
	bottom 0
	z-index 50
	.content
		display flex
		background #141d27
		width 100%
		height 100%
		font-size 0
		.content-left
			flex: 1
			.logo-wrapper
				display inline-block
				vertical-align top
				position relative
				top -10px
				padding 6px
				margin 0 12px
				width 56px
				height 56px
				box-sizing border-box
				border-radius 50%
				background #141d27
				.logo
					width 100%
					height 100%
					border-radius 50%
					background #2b343c
					display flex
					align-items center
					justify-content center
					&.highlight
						background rgb(0,160,220)
					.icon-shopping_cart
						font-size 24px
						color #80858aicon
						&.highlight
							color #fff
				.num
					position absolute
					top 0px
					right 0px
					width 24px
					height 16px
					line-height 16px
					text-align center
					background rgb(240,20,20)
					color #fff
					border-radius 16px
					font-size 9px
					box-shadow 0 4px 8px 0 rgba(0,0,0,0.4)
			.price
				display inline-block
				vertical-align top
				margin-top 12px
				padding-right 12px
				line-height 24px
				font-size 16px
				font-weight 700
				box-sizing border-box
				border-right 1px solid rgba(255,255,255,0.1)
				color rgba(255,255,255,0.4)
				&.highlight
					color #fff
			.desc
				display inline-block
				vertical-align top
				line-height 24px
				margin 12px 0 0 12px
				color rgba(255,255,255,0.4)
				font-size 12px
		.content-right
			flex 0 0 105px
			width 105px
			.pay
				font-size 12px
				font-weight 700
				height 48px
				line-height 48px
				text-align center
				color rgba(255,255,255,0.4)
				background #2b333b
				&.enough
					background #00b43c
					color #fff
	.ball-container
		.ball
			position fixed
			left 32px
			bottom 22px
			z-index 200
			transition: all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41)
			.inner
				width 16px
				height 16px
				border-radius 50%
				background rgb(0,160,220)
				transition all 0.4s linear
</style>