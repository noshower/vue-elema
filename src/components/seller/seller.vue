<template>
	<div class="seller" ref="seller">
		<div class="seller-content">
			<div class="overview">
				<h1 class="title">{{seller.name}}</h1>
				<div class="desc">
					<star :size="36" :score="seller.score"></star>	
					<span class="text">({{seller.ratingCount}})</span>		
					<span class="text">月售{{seller.sellCount}}</span>
				</div>
				<ul class="remark">
					<li class="block">
						<h2>起送价</h2>
						<div class="content">
							<span class="stress">{{seller.minPrice}}</span>元
						</div>
					</li>
					<li class="block">
						<h2>商家配送</h2>
						<div class="content">
							<span class="stress">{{seller.deliveryPrice}}</span>元
						</div>
					</li>
					<li class="block">
						<h2>平均配送时间</h2>
						<div class="content">
							<span class="stress">{{seller.deliveryTime}}</span>分钟
						</div>
					</li>
				</ul>
				<div class="favorite" @click="toggleFavorite($event)">
					<span class="icon-favorite" :class="{'active':favorite}"></span>
					<span class="text">{{favoriteText}}</span>
				</div>
			</div>
			<split></split>
			<div class="bulletin">
				<h1 class="title">公告与活动</h1>
				<div class="content-wrapper">
					<p class="content">{{seller.bulletin}}</p>
				</div>
				<ul v-if="seller.supports" class="supports">
					<li class="support-item" v-for="(item, index) in seller.supports">
						<span class="icon" :class="classMap[seller.supports[index].type]"></span>
						<span class="text">{{seller.supports[index].description}}</span>
					</li>
				</ul>
			</div>
			<split></split>
			<div class="pics">
				<h1 class="title">商家实景</h1>
				<div class="pic-wrapper" ref="pic-wrapper">
					<ul class="pic-list" ref="pic-list">
						<li class="pic-item" v-for="pic in seller.pics">
							<img :src="pic">
						</li>
					</ul>
				</div>
			</div>
			<split></split>
			<div class="info">
				<h1 class="title"></h1>
				<ul>
					<li class="info-item" v-for="info in seller.infos">{{info}}</li>
				</ul>
			</div>
		</div>
	</div>
</template>
<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import star from 'components/star/star'
  import split from 'components/split/split'
  export default {
  	props: {
  	  seller: Object	
  	},
  	data () {
  	  return {
        classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
        favorite: false
      }
  	},
  	computed: {
  	  favoriteText () {
  	  	return this.favorite ? '已收藏' : '未收藏'
  	  }
  	},
  	components: {
  	  star,
  	  split
  	},
  	watch: {
      'seller' () {
      	this.$nextTick(() => {
      	  this._initScroll()
      	  this._initPics()
      	})
      }
  	},
  	mounted () {
  	  this._initScroll()
  	  this._initPics()
  	},
  	methods: {
  	  _initScroll () {
  	  	if (!this.scroll) {
  	  	  this.scroll = new BScroll(this.$refs['seller'], {
  	  	    click: true
  	  	  })
  	  	} else {
  	  		this.scroll.refresh()
  	  	}
  	  },
  	  _initPics () {
  	  	if (this.seller.pics) {
  	  	  let picWidth = 3.2
  	  	  let margin = 0.16
  	  	  let width = (picWidth + margin) * this.seller.pics.length - margin
  	  	  this.$refs['pic-list'].style.width = width * (document.body.clientWidth / 10) + 'px'
  	  	  this.$nextTick(() => {
  	  	    if (!this.picScroll) {
  	  	      this.picScroll = new BScroll(this.$refs['pic-wrapper'], {
	  		    scrollX: true,
	  	        eventPassthrough: 'vertical'
  	  	      })
  	  	    } else {
  	  	    	this.picScroll.refresh()
  	  	    } 
  	  	  })
  	  	}
  	  },
  	  toggleFavorite (event) {
  	  	if (!event._constructed) {
  	  	  return 
  	  	}
  	  	this.favorite = !this.favorite
  	  }
  	}
  }
</script>
<style lang="scss">
	@import '../../common/scss/mixin.scss';
	.seller {
		position: absolute;
		top: pxToRem(174);
		bottom:0;
		left:0;
		width:100%;
		overflow: hidden;
		.overview {
			padding: pxToRem(18);
			position:relative;
			.title {
				margin-bottom:pxToRem(8);
				line-height: pxToRem(14);
				color:rgb(7,17,27);
				font-size: pxToRem(14);
			}
			.desc {
				padding-bottom:pxToRem(18);
				@include border-1px(rgba(7,17,27,0.1));
				font-size: 0;
				.star {
					display: inline-block;
					margin-right: pxToRem(8);
					vertical-align: top;
				}
				.text {
					margin-right: pxToRem(12);
					display: inline-block;
					vertical-align: top;
					font-size: pxToRem(10);
					color:rgb(77,85,93);
					line-height: pxToRem(18);
				}			
			}
			.remark {
				display: flex;
				padding-top: pxToRem(18);
				.block {
					flex: 1;
					text-align: center;
					border-right: 1px solid rgba(7,17,27,0.1);
					&:last-child {
					 	border:none;
					}
					h2 {
						margin-bottom: pxToRem(4);
						line-height: pxToRem(10);
						font-size: pxToRem(10);
						color:rgb(147,153,159);					
					}
					.content {
						line-height: pxToRem(24);
						font-size: pxToRem(10);
						color:rgb(7,17,27);
						.stress {
							font-size: pxToRem(24);
						}
					}
				}
			}
			.favorite {
				position: absolute;
				right: pxToRem(18);
				top: pxToRem(18);
				text-align: center;
				.icon-favorite {
					display: block;
					margin-bottom: pxToRem(4);
					line-height: pxToRem(24);
					font-size: pxToRem(24);
					color:#d4d6d9;
					&.active {
						color:rgb(240,20,2)
					}
				}
				.text {
					font-size: pxToRem(10);
					line-height: pxToRem(10);
					color: rgb(77,85,93);
				}
			}
		}
		.bulletin {
			padding:pxToRem(18) pxToRem(18) 0 pxToRem(18);
			.title {
				margin-bottom: pxToRem(8);
				line-height: pxToRem(14);
				color:rgb(7,17,27);
				font-size: pxToRem(14);
			}
			.content-wrapper {
				padding:0 pxToRem(12) pxToRem(16) pxToRem(12);
				@include border-1px(rgba(7,17,27,0.1));
				.content {
					line-height: pxToRem(24);
					font-size: pxToRem(12);
					color: rgb(240,20,20);
				}
			}
			.supports {
				.support-item {
					padding:pxToRem(16) pxToRem(12);
					@include border-1px(rgba(7,17,27,0.1));
					font-size: 0;
					&:last-child {
						@include border-none;
					}
				}
				.icon {
					display: inline-block;
					vertical-align: top;
					width:pxToRem(16);
					height: pxToRem(16);
					margin-right: pxToRem(6);
					background-size: pxToRem(16) pxToRem(16);
					background-repeat:no-repeat;
					&.decrease {
						@include bg-image('decrease_4');
					}
					&.discount {
						@include bg-image('discount_4');
					}
					&.guarantee {
						@include bg-image('guarantee_4');
					}
					&.invoice {
						@include bg-image('invoice_4');
					}
					&.special {
						@include bg-image('special_4');
					}
				}
				.text {
					line-height: pxToRem(16);
					font-size: pxToRem(12);
					color: rgb(7,17,27);
				}
			}
		}
		.pics {
			padding:pxToRem(18);
			.title {
				margin-bottom: pxToRem(12);
				line-height: pxToRem(14);
				color:rgb(7,17,27);
				font-size:pxToRem(14);
			}
			.pic-wrapper {
				width: 100%;
			    // overflow: hidden;
			    overflow-x: scroll;
				white-space: nowrap;
				height: pxToRem(90);
				.pic-list {
					font-size: 0;
					.pic-item {
						display: inline-block;
						margin-right: pxToRem(6);
						width: pxToRem(120);
						height: pxToRem(90);
						img {
							width: pxToRem(120);
							height: pxToRem(90);
						}
						&:last-child {
							margin-right: 0;
						}
					}
				}
			}
		}
		.info {
			padding:pxToRem(18) pxToRem(18) 0 pxToRem(18);
			.title {
				padding-bottom: pxToRem(12);
				line-height: pxToRem(14);
				@include border-1px(rgba(7,17,27,0.1));
				color:rgb(7,17,27);
				font-size: pxToRem(14);
			}
			.info-item {
				padding:pxToRem(16) pxToRem(12);
				line-height: pxToRem(16);
				@include border-1px(rgba(7,17,27,0.1));
				font-size: pxToRem(12);
				&:last-child {
					@include border-none;
				}
			}
		}
	}
</style>
