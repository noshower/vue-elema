<template>
	<div class="star" :class="starType">
		<span v-for="itemClass of itemClasses" :class="itemClass" class="star-item"></span>
	</div>
</template>
<script type="text/ecmascript-6">
  const LENGTH = 5
  const CLS_ON = 'on'
  const CLS_HALF = 'half'
  const CLS_OFF = 'off'
  export default {
    props: {
      size: {
        type: Number
      },
      score: {
        type: Number
      }
    },
    computed: {
      starType () {
        return 'star-' + this.size
      },
      itemClasses () {
        let result = []
        let score = Math.floor(this.score * 2) / 2
        let hasDecimal = score % 1 !== 0
        let interger = Math.floor(score)
        for (let i = 0; i < interger; i++) {
          result.push(CLS_ON)
        }
        if (hasDecimal) {
          result.push(CLS_HALF)
        }
        while (result.length < LENGTH) {
          result.push(CLS_OFF)
        }
        return result
      }
    }
  }
</script>
<style lang="scss">
@import '../../common/scss/mixin.scss';
	.star {
		font-size: 0;
		.star-item {
			display: inline-block;
			background-repeat: no-repeat;
		}
		&.star-48 {
			.star-item {
				width: pxToRem(20);
				height: pxToRem(20);
				margin-right: pxToRem(22);
				background-size:pxToRem(20) pxToRem(20);
				&:last-child{
					margin-right: 0;
				}
				&.on {
					@include bg-image('star48_on');
				}
				&.half {
					@include bg-image('star48_half');
				}
				&.off {
					@include bg-image('star48_off');
				}
			}
		}
		&.star-36 {
				.star-item {
				width: pxToRem(15);
				height: pxToRem(15);
				margin-right: pxToRem(6);
				background-size:pxToRem(15) pxToRem(15);
				&:last-child{
					margin-right: 0;
				}
				&.on {
					@include bg-image('star36_on');
				}
				&.half {
					@include bg-image('star36_half');
				}
				&.off {
					@include bg-image('star36_off');
				}
			}
		}
		&.star-24 {
				.star-item {
				width: pxToRem(10);
				height: pxToRem(10);
				margin-right: pxToRem(3);
				background-size:pxToRem(10) pxToRem(10);
				&:last-child{
					margin-right: 0;
				}
				&.on {
					@include bg-image('star24_on');
				}
				&.half {
					@include bg-image('star24_half');
				}
				&.off {
					@include bg-image('star24_off');
				}
			}
		}
	}
</style>
