<template>
	<div class="ratings" ref="ratings">
		<div class="ratings-content">
			<div class="overview">
				<div class="overview-left">
					<h1 class="score">{{seller.score}}</h1>
					<div class="title">综合评分</div>
					<div class="rank">高于周边商家{{seller.rankRate}}%</div>
				</div>
				<div class="overview-right">
					<div class="score-wrapper">
						<div class="title">服务态度</div>
						<star :size="36" :score="seller.serviceScore"></star>
						<span class="score">{{seller.serviceScore}}</span>
					</div>
					<div class="score-wrapper">
						<div class="title">商品评价</div>
						<star :size="36" :score="seller.foodScore"></star>
						<span class="score">{{seller.foodScore}}</span>
					</div>
					<div class="delivery-wrapper">
						<span class="title">送达时间</span>
						<span class="delivery">{{seller.deliveryTime}}分钟</span>
					</div>
				</div>
			</div>
			<split></split>
			<ratingselect v-on:ratingTypeSelect="typeSelect" v-on:contentToggle="toggleContent" :select-type="selectType" :only-content="onlyContent"  :ratings="ratings"></ratingselect>
			<div class="rating-wrapper">
				<ul>
					<li v-show="needShow(rating.rateType,rating.text)" v-for="rating in ratings" class="rating-item">
						<div class="avatar">
							<img :src="rating.avatar">
						</div>
						<div class="content">
							<h1 class="name">{{rating.username}}</h1>
							<div class="star-wrapper">
								<star :size="24" :score="rating.score"></star>
								<span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}</span>
							</div>
							<p class="text">{{rating.text}}</p>
							<div class="recommend" v-show="rating.recommend && rating.recommend.length">
								<span class="icon-thumb_up"></span>
								<span class="item" v-for="item in rating.recommend">{{item}}</span>
							</div>
							<div class="time">
								<span class="rateTime">{{rating.rateTime | formateDate}}</span>
							</div>
						</div>
					</li>
				</ul>
			</div>
		</div>
	</div>
</template>
<script type="text/ecmascript-6">
  import axios from 'axios'
  import BScroll from 'better-scroll'
  import star from 'components/star/star'
  import ratingselect from 'components/ratingselect/ratingselect'
  import split from 'components/split/split'
  import {formateDate} from 'common/js/date'
  const ALL = 2
  const ERR_OK = 0
  export default {
  	props: {
  	  seller: {
  	  	type: Object
  	  }
  	},
  	data () {
  	  return {
  	  	ratings: [],
  	    selectType: ALL,
  	    onlyContent: true
  	  }
  	},
  	created () {
      axios.get('/api/ratings').then((res) => {
      	res = res.data
      	if (res.errno === ERR_OK) {
      	  this.ratings = res.data
      	  this.$nextTick(() => {
      	  	this.scroll = new BScroll(this.$refs['ratings'], {
      	  	 click: true
      	  	})
      	  })
      	}
      })
  	},
  	methods: {
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
  	  star,
  	  ratingselect,
  	  split
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
	.ratings{
		position: absolute;
		top: pxToRem(174);
		bottom:0;
		left: 0;
		width: 100%;
		overflow: hidden;
		.overview {
			display: flex;
			padding: pxToRem(18) 0;
			.overview-left {
				width: pxToRem(137);
				padding-bottom:pxToRem(6);
				border-right: 1px solid rgba(7,17,27,0.1);
				text-align: center;
				.score {
					margin-bottom: pxToRem(6);
					line-height: pxToRem(28);
					font-size: pxToRem(24);
					color:rgb(255,153,0)
				}
				.title {
					margin-bottom: pxToRem(8);
					line-height: pxToRem(12);
					font-size: pxToRem(12);
					color:rgb(7,17,27);
				}
				.rank {
					line-height: pxToRem(10);
					font-size: pxToRem(10);
					color:rgb(147,153,159);

				}
			}
			.overview-right {
				flex: 1;
				padding-left:pxToRem(24);
				.score-wrapper {
					margin-bottom: pxToRem(8);
					font-size: 0;
					.title {
						display: inline-block;
						line-height: pxToRem(18);
						font-size: pxToRem(12);
						color:rgb(7,17,27);
					}
					.star {
						display: inline-block;
						vertical-align: top;
						margin:0 pxToRem(12);
					}
					.score {
						color: rgb(255,153,0);
						line-height: pxToRem(18);
						display: inline-block;
						vertical-align: top;
						font-size: pxToRem(12);
					}
				}
				.delivery-wrapper {
					font-size: 0;
					.title {
						line-height: pxToRem(18);
						font-size: pxToRem(12);
						color:rgb(7,17,27);
					}
					.delivery {
						margin-left: pxToRem(12);
						font-size: pxToRem(12);
						color: rgb(147,153,159);
					}
				}
			}
		}
		.rating-wrapper {
			padding:0 pxToRem(18);
			.rating-item {
				display: flex;
				padding:pxToRem(18) 0;
				@include border-1px(rgba(7,17,27,0.1));
				.avatar {
					flex: 0 0 pxToRem(28);
					width: pxToRem(28);
					margin-right: pxToRem(12);
					border-radius:50%;
					height: pxToRem(28);
					img {
						width:pxToRem(28);
						height: pxToRem(28);
					}
				}
				.content {
					position:relative;
					flex: 1;
					.name {
						line-height: pxToRem(12);
						font-size: pxToRem(10);
						color:rgb(7,17,27);
						margin-bottom: pxToRem(4);
					}
					.star-wrapper {
						margin-bottom: pxToRem(6);
						font-size: 0;
						.star {
							display: inline-block;
							vertical-align: top;
							margin-right: pxToRem(6);
						}
						.delivery {
							display: inline-block;
							vertical-align: top;
							line-height: pxToRem(12);
							font-size: pxToRem(10);
							color:rgb(147,153,159);
						}
					}
					.text {
						margin-bottom: pxToRem(8);
						line-height: pxToRem(18);
						color:rgb(7,17,27);
						font-size: pxToRem(12);
					}
					.recommend {
						line-height: pxToRem(16);
						font-size:0;
						.icon-thumb_up,.item{
							display: inline-block;
							margin:0 pxToRem(8) pxToRem(4) 0;
							font-size: pxToRem(9);
						}
						.icon-thumb_up {
							color:rgb(0,160,220);
						}
						.item  {
							padding:0 pxToRem(6);
							border:1px solid rgba(7,17,27,0.1);
							border-radius:1px;
							color:rgb(147,153,159);
							background: white;
						}
					}
					.time {
						position:absolute;
						top: 0;
						right: 0;
						line-height: pxToRem(12);
						font-size: pxToRem(10);
						color: rgb(147,153,159);
					}
				}
			}
		}
	}
</style>
