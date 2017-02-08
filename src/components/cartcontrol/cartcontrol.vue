<template>
	<div class="cartcontrol">
		<div class="cart-decrease icon-remove_circle_outline" v-show="food.count>0" @click.stop="decreaseCart($event)"></div>
		<div class="cart-count" v-show="food.count>0">{{food.count}}</div>
		<div class="cart-add icon-add_circle" @click.stop="addCart($event)"></div>
	</div>
</template>
<script type="text/ecmascript-6">
  import Vue from 'vue'
  export default {
    props: {
      food: {
      	type: Object
      }
    },
   	methods: {
   	  addCart (event) {
   	  	if (!event._constructed) {
   	  	  return false
   	  	}
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1)
        } else {
          this.food.count++
        }
   	  },
   	  decreaseCart (event) {
   	  	if (!event._constructed) {
   	  	  return false
   	  	}
        if (this.food.count) {
          this.food.count--
        }
   	  }
   	}
  }
</script>
<style lang="scss">
	@import '../../common/scss/mixin.scss';
	.cartcontrol {
		font-size: 0;
		.cart-decrease,.cart-add{
			display: inline-block;
			padding: pxToRem(6);
			line-height: pxToRem(24);
			font-size: pxToRem(24);
			color:rgb(0,160,220);
		}
		.cart-count {
			display: inline-block;
			vertical-align: top;
			width: pxToRem(12);
			padding-top:pxToRem(6);
			line-height: pxToRem(24);
			font-size: pxToRem(10);
			text-align: center;
			color: rgb(147,153,159);
		}
		.cart-add {
			display: inline-block;
		}
	}
</style>
