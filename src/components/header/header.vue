<template>
	<div class="header">
		<div class="content-wrapper">
			<div class="avatar">
				<img  :src="seller.avatar">
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
			</div>
			<div v-if="seller.supports" class="support-count" @click="showDetail">
				<span class="count">{{seller.supports.length}}个</span>
				<i class="icon-keyboard_arrow_right"></i>
			</div>
		</div>
		<div class="bulletin-wrapper" @click="showDetail">
			<span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
			<i class="icon-keyboard_arrow_right"></i>
		</div>
		<div class="background">
			<img :src="seller.avatar" width="100%" height="100%">
		</div>	
		<transition name="fade">
		<div v-show="detailShow" class="detail">
			<div class="detail-wrapper clearfix">
				<div class="detail-main">
					<h1 class="name">{{seller.name}}</h1>
					<div class="star-wrapper">
						<star  :size="48" :score="seller.score"></star>
					</div>
					<div class="title">
						<div class="line"></div>
						<div class="text">优惠信息</div>
						<div class="line"></div>
					</div>
					<ul v-if="seller.supports" class="supports">
						<li class="support-item" v-for="(item, index) in seller.supports">
							<span class="icon" :class="classMap[seller.supports[index].type]"></span>
							<span class="text">{{seller.supports[index].description}}</span>
						</li>
					</ul>
					<div class="title">
						<div class="line"></div>
						<div class="text">商家公告</div>
						<div class="line"></div>
					</div>
					<div class="bulletin">
						<p class="content">{{seller.bulletin}}</p>
					</div>	
				</div>
			</div>
			<div class="detail-close" @click="hideDetail">
				<i class="icon-close"></i>
			</div>
		</div>
		</transition>
	</div>
</template>
<script type="text/ecmascript-6">
  import star from 'components/star/star'
  export default {
    data () {
      return {
        classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
        detailShow: false
      }
    },
    props: {
      seller: {
        type: Object
      }
    },
    methods: {
      showDetail () {
        this.detailShow = true
      },
      hideDetail () {
        this.detailShow = false
      }
    },
    components: {
      star
    }
  }
</script>
<style lang="scss" style="stylesheet/scss">
	@import '../../common/scss/mixin.scss';
	.fade-enter-active,.fade-leave-active {
		transition: opacity 0.5s;
	}
	.fade-enter,.fade-leave-active {
		opacity: 0;
	}
	.header {
		color:#fff;
		position:relative;
		background:rgba(7,17,27,0.3);
		overflow: hidden;
		.content-wrapper {
			padding: pxToRem(24) pxToRem(12) pxToRem(18) pxToRem(24);
			position:relative;
			font-size: 0;
			.avatar{
				display: inline-block;
				vertical-align: top;
				img {
					height: pxToRem(64);
					width: pxToRem(64);
					border-radius:pxToRem(2);
				}
			}
			.content{
				display: inline-block;
				font-size: pxToRem(14);
				margin-left: pxToRem(16);
				.title {
					margin:pxToRem(2) 0 pxToRem(8) 0;
					.brand {
						height: pxToRem(18);
						width: pxToRem(30);
						display: inline-block;
						vertical-align: top;
						@include bg-image('brand');
						background-size:pxToRem(30) pxToRem(18);
						background-repeat:no-repeat;
					}
					.name {
						margin-left: pxToRem(6);
						font-size: pxToRem(16);
						line-height: pxToRem(18);
					}
				}
				.description {
					margin-bottom: pxToRem(10);
					line-height: pxToRem(12);
					font-size: pxToRem(12);
				}
				.support {
					font-size: 0;
					.icon {
						display: inline-block;
						vertical-align: top;
						width:pxToRem(12);
						height: pxToRem(12);
						margin-right: pxToRem(4);
						background-size: pxToRem(12) pxToRem(12);
						background-repeat:no-repeat;
						&.decrease {
							@include bg-image('decrease_1');
						}
						&.discount {
							@include bg-image('discount_1');
						}
						&.guarantee {
							@include bg-image('guarantee_1');
						}
						&.invoice {
							@include bg-image('invoice_1');
						}
						&.special {
							@include bg-image('special_1');
						}
					}
					.text {
						font-size: pxToRem(10);
						line-height: pxToRem(12);
					}
				}
			}
			.support-count{
				position: absolute;
				right: pxToRem(12);
				bottom:pxToRem(14);
				padding:0 pxToRem(8);
				height: pxToRem(24);
				line-height: pxToRem(24);
				border-radius:pxToRem(14);
				background-color:rgba(0,0,0,0.2);
				text-align: center;
				.count {
					vertical-align: top;
					font-size:pxToRem(10);
				}
				.icon-keyboard_arrow_right{
					font-size:pxToRem(10);
					line-height:pxToRem(24);
					margin-left: pxToRem(2);
				}

			}
		}
		.bulletin-wrapper {
			position:relative;
			height: pxToRem(28);
			line-height: pxToRem(28);
			padding:0 pxToRem(22) 0 pxToRem(12);
			white-space: nowrap;
			overflow: hidden;
			text-overflow: ellipsis;
			background-color:rgba(7,17,27,0.2);
			.bulletin-title {
				display: inline-block;
				vertical-align: top;
				margin-top: pxToRem(8);
				width: pxToRem(22);
				height: pxToRem(12);
				@include bg-image('bulletin');
				background-size:pxToRem(22) pxToRem(12);
				background-repeat:no-repeat;
			}
			.bulletin-text {
				font-size: pxToRem(10);
				vertical-align: top;
				margin:0 pxToRem(4);
			}
			.icon-keyboard_arrow_right {
				position:absolute;
				font-size: pxToRem(10);
				right: pxToRem(12);
				top: pxToRem(8);
			}
		}
		.background {
			position:absolute;
			top:0;
			left:0;
			width:100%;
			height: 100%;
			z-index: -1;
			filter:blur(10px);
		}
		.detail {
			position:fixed;
			z-index: 5;
			width: 100%;
			height: 100%;
			overflow: auto;
			background:rgba(7,17,27,0.8);
			top: 0;
			left: 0;
			backdrop-filter:blur(10px);
			.detail-wrapper {
				min-height: 100%;
				width: 100%;
				.detail-main {
					margin-top: pxToRem(64);
					padding-bottom:pxToRem(64);
					.name {
						line-height: pxToRem(16);
						text-align: center;
						font-size: pxToRem(16);
						font-weight: 700;
					}
					.star-wrapper {
						margin-top:pxToRem(18);
						padding:pxToRem(2) 0;
						text-align: center;
					}
					.title {
						width: 80%;
						margin:pxToRem(28) auto pxToRem(24) auto;
						display: flex;
						.line {
							flex:1;
							position:relative;
							top: -6px;
							border-bottom:1px solid rgba(255,255,255,0.2);
						}
						.text {
							padding:0 pxToRem(12);
							font-weight: 700;
							font-size: pxToRem(14);
						}
					}
					.supports {
						width: 80%;
						margin:0 auto;
						.support-item {
							padding:0 pxToRem(12);
							margin-bottom: pxToRem(12);
							font-size: 0;
							&:last-child {
								margin-bottom: 0
							}
							.icon {
								display: inline-block;
								width: pxToRem(16);
								height: pxToRem(16);
								vertical-align: top;
								margin-right: pxToRem(6);
								background-size:pxToRem(16) pxToRem(16);
								background-repeat:no-repeat;
								&.decrease {
									@include bg-image('decrease_2');
								}
								&.discount {
									@include bg-image('discount_2');
								}
								&.guarantee {
									@include bg-image('guarantee_2');
								}
								&.invoice {
									@include bg-image('invoice_2');
								}
								&.special {
									@include bg-image('special_2');
								}
							}
							.text {
								line-height: pxToRem(16);
								font-size: pxToRem(12);
							}
						}
					}
					.bulletin {
						width:80%;
						margin:0 auto;
						.content {
							padding:0 pxToRem(12);
							line-height: pxToRem(24);
							font-size: pxToRem(12);
						}
					}
				}
			}
			.detail-close {
				position:relative;
				width: pxToRem(32);
				height: pxToRem(32);
				margin:pxToRem(-64) auto 0 auto;
				clear:both;
				font-size: pxToRem(32);
			}
		}
	}
</style>

