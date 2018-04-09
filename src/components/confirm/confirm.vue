<template>
	<div class="confirm" v-show="showFlag">
		<div class="dialog">
			<div class="text">
				<h1 class="title">{{title}}</h1>
				<p class="description" v-show="description">{{description}}</p>
			</div>
			<div>
				<span class="ok" @click="clickOk()">{{textOk}}</span>
				<span class="cancel" @click="clickCancel()">{{textCancel}}</span>
			</div>
		</div>
	</div>
</template>

<script type="text/ecmascript-6">
	export default {
		props: {
			title: {
				type: String,
				default: '确定吗?'
			},
			description: {
				type: String
			},
			textOk: {
				type: String,
				default: '确认'
			},
			textCancel: {
				type: String,
				default: '取消'
			}
		},
		data () {
			return {
				showFlag: false
			};
		},
		methods: {
			open () {
				this.showFlag = true;
			},
			clickOk () {
				this.$emit('ok');
				this.showFlag = false;
			},
			clickCancel () {
				this.$emit('cancel');
				this.showFlag = false;
			}
		}
	};
</script>

<style lang="stylus" rel="stysheet/stylus">
@import "../../common/stylus/mixin"

.confirm
	position fixed
	top 0
	left 0
	width 100%
	height 100%
	background rgba(7,17,27,0.1)
	z-index 1000
	display flex
	align-items center
	justify-content center
	.dialog
		min-width 250px
		background #fff
		border-radius 5px
		font-size 0
		.text
			color rgb(7,17,27)
			.title
				font-size 16px
				font-weight 700
				line-height 50px
				text-align center
				border-1px(rgba(7,17,27,0.2))
			.description
				padding 15px 8%
				line-height 20px
				font-size 14px
				border-1px(rgba(7,17,27,0.2))
		.ok, .cancel
			width 50%
			font-size 14px
			display inline-block
			line-height 40px
			text-align center
			box-sizing border-box
			color: rgb(0,160,220)
		.ok
			padding-left -1px
			border-right 1px solid rgba(7,17,27,0.2)
</style>
