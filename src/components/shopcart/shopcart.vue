<template>
	<div>
   <div class="shopcart">
    <div class="content" @click="toggleList">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{'highlight':totalCount>0}">
            <i class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></i>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}元</div>
        <div class="desc">另需配送费{{deliveryPrice}}元</div>
      </div>
      <div class="content-right">
        <div class="pay" :class="payClass" @click.stop="pay">
          {{payDesc}}
        </div>
      </div>
    </div>
    <transition name="fold">
      <div class="shopcart-list" v-show="listShow">
      <div class="list-header">
        <h1 class="title">购物车</h1>
        <span class="empty" @click="empty">清空</span>
      </div>
      <div class="list-content" ref="list-content">
        <ul>
          <li class="food" v-for="food in selectFoods">
            <span class="name">{{food.name}}</span>
            <div class="price">
              <span>￥{{food.price * food.count}}</span>
            </div>
            <div class="cartcontrol-wrapper">
              <cartcontrol :food="food"></cartcontrol>
            </div>
          </li>
        </ul>
      </div>
    </div>
    </transition> 
  </div> 
  <transition name="fade">
    <div @click="hideList" class="list-mask" v-show="listShow"></div> 
  </transition>
  </div>
</template>
<script type="text/ecmascript-6">
  import cartcontrol from 'components/cartcontrol/cartcontrol'
  import BScroll from 'better-scroll'
  export default {
    props: {
    	selectFoods: {
    	  type: Array,
    	  default () {
    	  	return [{
    	  	  count: 2,
    	  	  price: 10
    	  	}]
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
        fold: true
      }
    },
    computed: {
    	totalPrice () {
    	  let total = 0
    	  this.selectFoods.forEach((food) => {
    	  	total += food.price * food.count
    	  })
    	  return total 
    	},
    	totalCount () {
    	  let count = 0
    	  this.selectFoods.forEach((food) => {
    	  	count += food.count
    	  })
    	  return count
    	},
    	payDesc () {
    	  if (this.totalPrice === 0) {
    	  	return `￥${this.minPrice}元起送`
    	  } else if (this.totalPrice < this.minPrice) {
    	  	let diff = this.minPrice - this.totalPrice
    	  	return `还差￥${diff}元起送`
    	  } else {
    	  	return '去结算'
    	  }
    	},
    	payClass () {
    	  if (this.totalPrice < this.minPrice) {
    	  	return 'not-enough'
    	  } else {
    	  	return 'enough'
    	  }
    	},
      listShow () {
        if (!this.totalCount) {
          this.fold = true
          return false
        }
        let show = !this.fold
        if (show) {
          this.$nextTick(() => {
            if (!this.scroll) {
              this.scroll = new BScroll(this.$refs['list-content'], {
                click: true
              })
            } else {
              this.scroll.refresh()
            }
          })
        }
        return show 
      }
    },
    methods: {
      toggleList () {
        if (!this.totalCount) {
          return false
        }
        this.fold = !this.fold
      },
      empty () {
        this.selectFoods.forEach((food) => {
          food.count = 0
        })
      },
      hideList () {
        this.fold = true
      },
      pay () {
        if (this.totalPrice < this.minPrice) {
          return false
        }
        window.alert(`支付${this.totalPrice}元`)
      }
    },
    components: {
      cartcontrol
    }
  }
</script>
<style lang="scss">
 @import '../../common/scss/mixin.scss';
 .fold-enter-active,.fold-leave-active{
      transition:opacity 0.5s;
    }
  .fold-enter,.fold-leave-active {
      opacity: 0;
  }
 .shopcart {
 	position: fixed;
 	left: 0;
 	bottom: 0;
 	z-index: 3;
 	width:100%;
 	height: pxToRem(48);
 	.content {
 		display: flex;
 		background: #141d27;
 		font-size: 0;
 		.content-left{
 			flex:1;
 			.logo-wrapper {
 				display: inline-block;
 				position:relative;
 				top:pxToRem(-10);
 				margin:0 pxToRem(12);
 				padding: pxToRem(6);
 				width: pxToRem(56);
 				height: pxToRem(56);
 				box-sizing:border-box;
 				vertical-align: top;
 				border-radius:50%;
 				background:#141d27;
 				.logo {
 					width: 100%;
 					height: 100%;
 					border-radius:50%;
 					background:#2b343c;
 					text-align: center;
 					&.highlight {
 						background:rgb(0,160,220)
 					}
 					.icon-shopping_cart {
 						font-size: pxToRem(24);
 						color: #80858a;
 						line-height: pxToRem(44);
 						&.highlight {
 							color: #fff;
 						}
 					}
 				}
 				.num {
 					position:absolute;
 					top: 0;
 					right: 0;
 					width:pxToRem(24);
 					height: pxToRem(16);
 					line-height: pxToRem(16);
 					text-align: center;
 					border-radius:pxToRem(16);
 					font-size: pxToRem(9);
 					font-weight: 700;
 					color:#fff;
 					background:rgb(240,20,20);
 					box-shadow:0 4px 8px 0 rgba(0,0,0,0.4);
 				}
 			}
 			.price{
 				display: inline-block;
 				vertical-align: top;
 				line-height: pxToRem(24);
 				margin-top: pxToRem(12);
 				box-sizing:border-box;
 				padding-right:pxToRem(12);
 				border-right:1px solid rgba(255,255,255,0.1);
 				font-size:pxToRem(16);
 				font-weight: 700;
 				color:rgba(255,255,255,0.4);
 				&.highlight {
 					color:#fff;
 				}
 			}
 			.desc{
 				display: inline-block;
 				vertical-align: top;
 				line-height: pxToRem(24);
 				margin: pxToRem(12) 0 0  pxToRem(12);
 				line-height: pxToRem(24);
 				color:rgba(255,255,255,0.4);
 				font-size: pxToRem(10);
 			}
 		}
 		.content-right{
 			flex: 0 0 pxToRem(105);
 			width: pxToRem(105);
 			.pay {
 				height: pxToRem(48);
 				line-height: pxToRem(48);
 				text-align: center;
 				font-size: pxToRem(12);
 				color:rgba(255,255,255,0.4);
 				font-weight: 700;
 				background:#2b333b;
 				&.not-enough{
 					background:#2b333b;
 				}
 				&.enough {
 					background:#00b43c;
 					color: #fff;
 				}
 			}
 		}
 	}
  .shopcart-list {
    position:absolute;
    left:0;
    bottom:pxToRem(48);
    z-index: -1;
    width: 100%;
    .list-header {
      height: pxToRem(40);
      line-height: pxToRem(40);
      padding:0 pxToRem(18);
      background:#f3f5f7;
      border-bottom:1px solid rgba(7,17,27,0.1);
      .title {
        float: left;
        font-size: pxToRem(14);
        color: rgb(7,17,27)
      }
      .empty {
        float: right;
        font-size: pxToRem(12);
        color: rgb(0,160,220);
      }
    }
    .list-content {
      padding:0 pxToRem(18);
      max-height: pxToRem(217);
      background:#fff;
      overflow:hidden;
      .food {
        position:relative;
        padding:pxToRem(12) 0;
        box-sizing:border-box;
        @include border-1px(rgba(7,17,27,0.1));
        .name {
          line-height:pxToRem(24);
          font-size:pxToRem(14);
          color:rgb(7,17,27);
        }
        .price {
          position:absolute;
          right: pxToRem(90);
          bottom:pxToRem(12);
          line-height: pxToRem(24);
          font-size: pxToRem(14);
          font-weight: 700;
          color:rgb(240,20,20);
        }
        .cartcontrol-wrapper {
          position:absolute;
          right: 0;
          bottom:pxToRem(6);
        }
      }
    }
  }
 }
 .list-mask {
  position:fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 2;
  background:rgba(7,17,27,0.6);
 }
</style>
