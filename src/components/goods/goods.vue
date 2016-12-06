<template>
  <div class="goods">
      <div class="menu-wrapper" v-el:menu-wrapper>
      	<ul>
      		<li v-for="item in goods" class="menu-item" :class="{'current': currentIndex===$index}" @click="selectIndex($index)">
      			<span class="text">
      				<span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
      			</span>
      			
      		</li>
      	</ul>
      </div>
      <div class="foods-wrapper" v-el:good-wrapper>
      	<ul>
      		<li v-for="good in goods" class="food-list food-list-hook">
      			<h1 class="title">{{good.name}}</h1>
      			<ul>
      				<li v-for="food in good.foods" class="food-item">
      					<div class="icon">
      						<img :src="food.icon" width="57" height="57">
      					</div>
      					<div class="content">
      						<h2 class="name">{{food.name}}</h2>
      						<p class="desc">{{food.description}}</p>
      						<div class="extra">
      							<span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
      						</div>
      						<div class="price">
      							<span class="now">¥{{food.price}}</span>
      							<span class="old" v-show="food.oldPrice">¥{{food.oldPrice}}</span>
      						</div>
      					</div>
      				</li>
      			</ul>
      		</li>
      	</ul>
      </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'
  export default{
  	props:{
  		seller:{
  			type:Object
  		}
  	},
  	data(){
  		return {
  			classMap:[],
  			goods:{},
  			listHeight:[],
  			scrollY: 0
  		}
  	},
  	computed: {
  		currentIndex(){
  			for(let i=0; i<this.listHeight.length; i++){
  				let height1 = this.listHeight[i]
  				let heigth2 = this.listHeight[i+1]
  				if(!heigth2 || this.scrollY >= height1 && this.scrollY <heigth2){
  					return i
  				}
  			}
  			return 0
  		}
  	},
  	created() {
  		this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
  		const ERR_OK = 0
  		this.$http.get('/api/goods').then((response) => {
            response = response.body
             if (response.errcode === ERR_OK) {
                this.goods = response.data
                this.$nextTick(() => {
                	this._initScroll()
                	this._calcHeight()
                })
            }
      	})
  	},
  	methods: {
  		selectIndex(index){
  			let items = this.$els.goodWrapper.getElementsByClassName('food-list-hook')
  			let el = items[index]
  			this.goodScroll.scrollToElement(el,300)
  		},
  		_initScroll() {
  			this.menuScroll = new BScroll(this.$els.menuWrapper, {
  				click: true
  			})
  			this.goodScroll = new BScroll(this.$els.goodWrapper, {
  				probeType: 3
  			})
  			this.goodScroll.on('scroll',(pos)=>{
  				this.scrollY = Math.abs( Math.round(pos.y))
  			})
  		},
  		_calcHeight() {
  			let items = this.$els.goodWrapper.getElementsByClassName('food-list-hook')
  			let height = 0
  			this.listHeight.push(height)
  			for (let i = 0; i < items.length; i++){
  				height += items[i].clientHeight
  				this.listHeight.push(height)
  			}
  			console.log('11' + this.listHeight)
  		}
  	}
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin.styl'
.goods
	position:absolute
	top:174px
	bottom:46px
	display:flex
	width:100%
	overflow:hidden
	.menu-wrapper
		flex:0 0 80px
		width:80px
		background:#f3f5f7
		.menu-item
			display:table
			height:54px
			width:56px
			color:rgb(77,85,93)
			font-size:12px
			line-height:14px
			padding:0 12px
			&.current
				position:relative
				z-index:10
				margin-top:1px
				background:#fff
				font-weight:700
				.text
					border-none()
			.icon
				display:inline-block
				vertical-align:top
				width:12px
				height:12px
				margin-right:2px
				background-size:12px 12px
				background-repeat:no-repeat
				&.decrease
					bg-image('decrease_3')
				&.discount
					bg-image('discount_3')
				&.guarantee
					bg-image('guarantee_3')
				&.invoice
					bg-image('invoice_3')
				&.special
					bg-image('special_3')
			.text
				display:table-cell
				width:56px
				vertical-align:middle
				border-1px(rgba(7,17,27,0.2))
				font-size:12px
				&:last-child
					border-none()
	.foods-wrapper
		flex:1
		.title
			padding-left:14px
			height: 26px
			line-height: 26px
			border-left: 2px solid #d9dde1
			font-size: 12px
			color: rgb(147, 153, 159)
			background: #f3f5f7
		.food-item
			display: flex
			margin: 18px
			padding-bottom:18px
			border-1px(rgba(7,17,27,0.1))
			&:last-child
				border-none()
				margin-bottom:0
			.icon
				flex: 0 0 57px
				margin-right: 10px
			.content
				flex:1
				.name
					margin: 2px 0 8px 0
					height: 14px
					line-height: 14px
					font-size: 14px
					color: rgb(7, 17, 27)
			.desc,.extra
				line-height: 10px
				font-size: 10px
				color: rgb(147, 153, 159)
			.desc
				line-height: 12pxq
				margin-bottom: 8px
			.extra
				.count
					margin-right:8px
			.price
				font-weight: 700
				line-height: 24px
				.now
					font-size:14px
					color:rgb(240,20,20)
					margin-right: 8px
				.old
					text-decoration: line-through
					font-size: 10px
					color: rgb(147, 153, 159)
			
</style>
