<template>
	<view class="cart-container" v-if="cart.length !== 0">
		<!-- 收货地址组件 -->
		<my-address></my-address>
		<!-- 商品列表标题区域-->
		<view class="cart-title">
			<!-- 左侧的图标 -->
			<uni-icons type="shop" size="18"></uni-icons>
			<!-- 右侧的文本 -->
			<text class="cart-title-text">购物车</text>
		</view>

		<!-- 循环渲染购物车中的商品信息 -->

		<uni-swipe-action>
			<block v-for="(goods, i) in cart" :key="i">
				<uni-swipe-action-item :right-options="options" @click="swipeItemClickHanlder(goods)">
					<my-goods :goods="goods" :showRadio="true" :showNum="true" @radio-change="radioChangeHandler"
						@num-change="numberChangeHanlder"></my-goods>
				</uni-swipe-action-item>
			</block>
		</uni-swipe-action>
		
		<!-- 使用自定义结算组件-->
		<my-settle></my-settle>

	</view>
	
	<view class="empty-cart" v-else>
		<image src="/static/logo.png" class="empty-img"></image>
		<text class="tip-text">空空如也</text>
	</view>
	
</template>

<script>
	import badgeMix from '@/mixins/tabbar-badge.js';
	import {
		mapState,
		mapMutations
	} from 'vuex';

	export default {

		mixins: [badgeMix],

		computed: {
			...mapState('m_cart', ['cart'])
		},

		data() {
			return {
				options: [{
					text: '删除',
					style: {
						backgroundColor: '#C00000'
					}
				}]
			};
		},

		methods: {

			...mapMutations('m_cart', ['updateGoodsState', 'updateGoodsCount','removeGoodsById']),

			radioChangeHandler(e) {
				this.updateGoodsState(e)
			},

			numberChangeHanlder(e) {
				this.updateGoodsCount(e)
			},
			
			swipeItemClickHanlder(e){
				this.removeGoodsById(e.goods_id)
			}

		}

	}
</script>

<style lang="scss">
	
	.cart-container {
		padding-bottom: 50px;
		
	}
	.cart-title {

		height: 40px;
		display: flex;
		align-items: center;
		padding-left: 5px;
		border-bottom: 1px solid #efefef;

		.cart-title-text {

			font-size: 14px;
			margin-left: 10px;

		}
	}
	
	
	.empty-cart {
		display: flex;
		flex-direction: column;
		align-items: center;
		padding-top: 150px;
		
		.empty-img {
			width: 90px;
			height:90px;
		}
		
		.tip-text {
			font-size: 12px;
			color: gray;
			margin-top: 15px;
		}
		
	}
</style>
