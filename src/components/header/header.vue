<template>
  <div class="header">
    <div class="content-wrapper">
    	<div class="avatar">
    		<img width="64" height="64" :src="seller.avatar">
    	</div>
    	<div class="content">
    		<div class="title">
    			<span class="brand"></span>
    			<span class="name">{{seller.name}}</span>
    		</div>
    		<div class="description">
    			{{seller.description}}/{{seller.deliveryTime}}分钟送达
    		</div>
    		<div v-if="seller.supports" class="support">
    			<span class="icon" :class="classMap[seller.supports[0].type]"></span>
    			<span class="text">{{seller.supports[0].description}}</span>
    		</div>
    		<div v-if="seller.supports" class="support-count" @click="showdetails">
    			<span class="count">{{seller.supports.length}}</span>
    			<i class="icon-keyboard_arrow_right"></i>
    		</div>
    	</div>
    </div>
    <div class="bulletin-wrapper" @click="showdetails">
    	<span class="bullentin-title"></span><span class="bullentin-text">{{seller.bulletin}}</span>
    	<i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
    		<img :src="seller.avatar" width="100%" height="100%">
    </div>
    <div v-show="detailshow" class="detail">
    	<star :score="seller.score" :size="48"></star>
    </div>
  </div>
</template>

<script>
  import star from 'components/star/star'
  export default {
  	props: {
  		seller: {
  			type: Object
  		}
  	},
  	data() {
  		return {
  			detailshow: false
  		}
  	},
  	methods: {
  		showdetails() {
  			this.detailshow = !this.detailshow
  		}
  	},
  	created() {
  		this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
  	},
  	components: {
  		star
  	}
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin.styl" 
.header
	position:relative
	color:white
	background:rgba(7,17,27,0.5)
	overflow:hidden
	.content-wrapper
		position:relative
		padding: 24px 12px 18px 24px
		font-size: 0
		.avatar
			display:inline-block
			vertical-align:top
			img
			border-radius:2px
		.content
			display:inline-block
			margin-left:16px
			font-size:12px
			.title
				margin: 2px 0 8px 0
				.brand
					display:inline-block
					vertical-align:top
					width:30px
					height:18px
					bg-image('brand')
					background-size: 30px 18px
					background-repeat: no-repeat
				.name
					font-size:16px
					font-bold:bold
					line-height:16px	
			.description
				margin-bottom:10px
				font-size:12px
				line-height:12px
			.support
				.icon
					display:inline-block
					vertical-align:top
					width:12px
					height:12px
					margin-right:4px
					background-size:12px 12px
					background-repeat:no-repeat
					&.decrease
						bg-image('decrease_1')
					&.discount
						bg-image('discount_1')
					&.guarantee
						bg-image('guarantee_1')
					&.invoice
						bg-image('invoice_1')
					&.special
						bg-image('special_1')
				.text
					font-size:10px
					line-height:12px
		.support-count
			position:absolute
			right:12px
			bottom:14px
			padding: 0 8px
			height:24px
			line-height:24px
			border-radius: 24px
			background: rgba(0,0,0.0.2)
			text-align:content-wrapper
			.count
				vertical-align:top
				font-size:10px
			.icon-keyboard_arrow_right
				margin-left:2px
				line-height:24px
				font-size:10px
	.bulletin-wrapper
		position:relative
		height:28px
		line-height:28px
		padding:0 22px 0 12px
		white-space:nowrap
		overflow:hidden
		text-overflow:ellipsis
		font-size:10px
		.bullentin-title
			vertical-align:top
			margin-top:8px
			display:inline-block
			width:22px
			height:12px	
			bg-image('bulletin')
			background-size:22px 12px
			background-repeat:no-repeat
		.bullentin-text
			vertical-align:top
			margin:0 2px
			font-size:10px
		.icon-keyboard_arrow_right
			position:absolute
			right:12px
			top:8px
			font-size:10px
	.background
		position:absolute
		top:0
		left:0
		width:100%
		height:100%
		background-repeat:no-repeat
		z-index:-1
		filter:blur(10px)
	.detail
		position:fixed
		top:0
		left:0
		z-index:199
		width:100%
		height:100%
		overflow:hidden
		background:rgba(7,17,27,0.8)

</style>
