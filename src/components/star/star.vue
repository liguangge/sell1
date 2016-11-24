<template>
	<div class="star" :class="startType">
		<span v-for="itemClass in itemClasses" :class="itemClass" class="star-item" track-by="$index"></v-for>
	</div>
</template>

<script>
  const LENGTH = 5
  const CLS_ON = 'on'
  const CLS_HALF = 'half'
  const CLS_OFF = 'off'
  export default{
  	props: {
  		size: {
  			type: Number
  		},
  		score: {
  			type: Number
  		}
  	},
  	computed: {
  		startType() {
  			return 'star-' + this.size
  		},
  		itemClasses() {
  			let result = []
  			let score = Math.floor(this.score * 2) / 2
  			let hasDecimal = score % 1 !== 0
  			let intScore = Math.floor(score)
  			console.log(intScore.toString())
  			for (let i = 0; i < intScore; i++) {
  				result.push(CLS_ON)
  			}
  			if (hasDecimal) {
  				result.push(CLS_HALF)
  			}
  			for (let i = 0, max = LENGTH - result.length; i < max; i++) {
  				result.push(CLS_OFF)
  			}
  			console.log(result)
  			return result
  		}
  	}
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin.styl'
.star
	font-size:0
	.star-item
		display:inline-block
		background:no-repeat
	&.star-48
		width:100%
	  	.star-item
			width:20px
			height:20px
			background-size:20px 20px
			margin-right:22px
			&:last-child
				margin-right:0
			&.on
				color:red
				bg-image('star48_on')
			&.half
				bg-image('star48_half')
			&.off
				bg-image('star48_off')
	&.star-36
		.star-item
		width:15px
		height:15px
		background-size:15px 15px
		margin-right:6px
		&.on
			bg-image('star36_on')
		&.half
			bg-image('star36_half')
		&.off
			bg-image('star36_off')
		&.last-child
			margin-right:0
	&.star-24
		.star-item
		width:10px
		height:10px
		background-size:10px 10px
		margin-right:3px
		&.on
			bg-image('star24_on')
		&.half
			bg-image('star24_half')
		&.off
			bg-image('star24_off')
		&.last-child
			margin-right:0
</style>
