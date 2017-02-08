<template>
  <transition name="move">
  	<div v-show="showFlag" class="food" ref="food">
  		<div class="food-content">
  			<div class="image-header">
  				<img :src="food.image">
  				<div class="back" @click="hide">
  					<i class="icon-arrow_lift"></i>
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
  				<div class="cartcontrol-wrapper">
  					<cartcontrol :food="food"></cartcontrol>
  				</div>
  				<div @click.stop="addFirst($event)" class="buy" v-show="!food.count || food.count ===0">加入购物车</div>
  			</div>
  			<split v-show="food.info"></split>
  			<div class="info" v-show="food.info">
  				<h1 class="title">商品信息</h1>
  				<p class="text">{{food.info}}</p>
  			</div>
  			<split></split>
  			<div class="rating">
  				<h1 class="title">商品评价</h1>
  				<ratingselect v-on:ratingTypeSelect="typeSelect" v-on:contentToggle="toggleContent" :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="food.ratings"></ratingselect>
  				<div class="rating-wrapper">
  					<ul v-show="food.ratings && food.ratings.length">
  						<li v-show="needShow(rating.rateType,rating.text)" v-for="rating in food.ratings" class="rating-item">
  							<div class="user">
  								<span class="name">{{rating.username}}</span>
  								<img :src="rating.avatar" class="avatar"/>
  							</div>
  							<div class="time">{{rating.rateTime | formateDate}}</div>
  							<p class="text">
  								<span :class="{'icon-thumb_up':rating.rateType === 0,'icon-thumb_down':rating.rateType === 1}"></span>{{rating.text}}
  							</p>
  						</li>
  					</ul>
  					<div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
  				</div>
  			</div>
  		</div>
 	</div>
  </transition>
</template>
<script type="text/ecmascript-6">
  import Vue from 'vue'
  import BScroll from 'better-scroll'
  import cartcontrol from 'components/cartcontrol/cartcontrol'
  import split from 'components/split/split'
  import ratingselect from 'components/ratingselect/ratingselect'
  import {formateDate} from 'common/js/date'
  // const POSITIVE = 0
  // const NEGATIVE = 1
  const ALL = 2
  export default {
  	props: {
  	  food: {
  	  	type: Object
  	  }
  	},
  	data () {
  	  return {
  	  	showFlag: false,
  	  	selectType: ALL,
  	  	onlyContent: true,
  	  	desc: {
  	  		all: '全部',
  	  		positive: '推荐',
  	  		negative: '吐槽'
  	  	}
  	  }
  	},
  	methods: {
  	  show () {
  	  	this.showFlag = true
  	  	this.selectType = ALL
  	  	this.onlyContent = true
  	  	this.$nextTick(() => {
  	  	  if (!this.scroll) {
  	  	    this.scroll = new BScroll(this.$refs['food'], { 
  	  	      click: true
  	  	    })
  	  	  } else {
  	  	  	this.scroll.refresh()  	  	  
  	  	  }
  	  	})
  	  },
  	  hide () {
  	  	this.showFlag = false
  	  },
  	  addFirst (event) {
  	    if (!event._constructed) {
  	    	return 
  	    }
  	    Vue.set(this.food, 'count', 1)
  	  },
  	  needShow (type, text) {
  	  	if (this.onlyContent && !text) {
  	  	  return
  	  	}
  	  	if (this.selectType === ALL) {
  	  	  return true 
  	  	} else {
  	  	  return type === this.selectType
  	  	}
  	  },
  	  typeSelect (type) {
  	  	this.selectType = type
  	  	this.$nextTick(() => {
  	  		this.scroll.refresh()
  	  	})
  	  },
  	  toggleContent (onlyContent) {
  	  	this.onlyContent = !onlyContent
  	  	this.$nextTick(() => {
  	  		this.scroll.refresh()
  	  	})
  	  }
  	},
  	components: {
  	  cartcontrol,
  	  split,
  	  ratingselect
  	},
  	filters: {
  	  formateDate (time) {
  		let date = new Date(time)
  		return formateDate(date, 'yyyy-MM-dd hh:mm')
  	  }
  	}
  }
</script>
<style lang="scss">
	@import '../../common/scss/mixin.scss';
	.move-enter-active, .move-leave-active {
  		transition: all 0.2s linear
	}
	.move-enter, .move-leave-active {
  		transform: translate3d(100%,0,0)
	}
	.food {
		position: fixed;
		left: 0;
		top: 0;
		bottom:pxToRem(48);
		z-index: 2;
		width: 100%;
		background: white;
		.food-content {
			.image-header {
				position:relative;
				width:100%;
				height: 0;
				padding-top:100%;
				img {
				position:absolute;
				top:0;
				left:0;
				width:100%;
				height: 100%;
				}
				.back {
					position:absolute;
					top:pxToRem(10);
					left:0;
					.icon-arrow_lift {
					display: block;
					padding: pxToRem(10);
					font-size: pxToRem(20);
					color:white;
					}
				}
			}
			.content {
				padding:pxToRem(18);
				position:relative;
				.title {
					line-height: pxToRem(14);
					margin-bottom: pxToRem(8);
					font-size: px(14);
					font-weight:700;
					color:rgb(7,17,27);
				}
				.detail {
					margin-bottom: pxToRem(18);
					line-height: pxToRem(10);
					height: pxToRem(10);
					font-size: 0;
					.sell-count,.rating {
						font-size: pxToRem(10);
						color: rgb(147,153,159);
					}
					.sell-count {
						margin-right: pxToRem(12);
					}
				}
				.price {
					font-weight: 700;
					line-height: pxToRem(24);
					.now {
						margin-right: pxToRem(18);
						font-size: pxToRem(14);
						color: rgb(240,20,20);
					}
					.old {
						text-decoration: line-through;
						font-size: pxToRem(10);
						color: rgb(147,153,159);
					}
				}
				.cartcontrol-wrapper {
					position:absolute;
					right: pxToRem(12);
					bottom:pxToRem(12);
				}
				.buy {
					position:absolute;
					right: pxToRem(18);
					bottom: pxToRem(18);
					z-index: 10;
					height: pxToRem(24);
					line-height: pxToRem(24);
					padding:0 pxToRem(12);
					box-sizing:border-box;
					font-size: pxToRem(10);
					border-radius:pxToRem(12);
					color:#fff;
					background:rgb(0,160,200);
				}
			}
			.info {
				padding:pxToRem(18);
				.title {
					line-height: pxToRem(14);
					font-size: pxToRem(14);
					margin-bottom: pxToRem(6);
					color:rgb(7,17,27);
				}
				.text {
					line-height: pxToRem(24);
					padding:0 pxToRem(8);
					font-size: pxToRem(12);
					color:rgb(77,85,94);
				}
			}
			.rating {
				padding-top: pxToRem(18);
				.title {
					line-height: pxToRem(14);
					font-size: pxToRem(14);
					margin-left: pxToRem(18);
					color:rgb(7,17,27);
					
				}
				.rating-wrapper {
					padding:0 pxToRem(18);
					.rating-item {
						position:relative;
						padding:pxToRem(16) 0;
						@include border-1px(rgba(7,17,27,0.1));
						.user {
							position:absolute;
							right: 0;
							top:pxToRem(16);
							line-height: pxToRem(12);
							font-size: 0;
							.name {
								display: inline-block;
								vertical-align: top;
								font-size: 10px;
								color:rgb(147,153,159);
								margin-right: pxToRem(6);
							}
							.avatar {
								height: pxToRem(12);
								width: pxToRem(12);
								border-radius:50%;
							}

						}
						.time {
							line-height: pxToRem(12);
							font-size: pxToRem(10);
							color: rgb(147,153,159);
							margin-bottom: pxToRem(6);
						}
						.text {
							line-height: pxToRem(16);
							font-size: pxToRem(12);
							color: rgb(7,17,27);
							.icon-thumb_down,.icon-thumb_up {
								line-height: pxToRem(16);
								margin-right: pxToRem(4);
								font-size: pxToRem(12);
							}
							.icon-thumb_up {
								color:rgb(0,169,220);
							}
							.icon-thumb_down {
								color:rgb(147,153,159);
							}
						}
					}
					.no-rating {
						padding:pxToRem(16) 0;
						font-size: pxToRem(12);
						color: rgb(147,153,159);
					}
				}
			}
		}		
	}
</style>
