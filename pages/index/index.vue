<!-- 首页
	admin：林轩
 -->
<template>
	<view>
		<view class="topbar">
			百宝袋
			<text class="subtitles">{{subtitles}}</text>
		</view>
		
		<u-cell-group title="日常">
			<view v-for="(item,index) in itemBox">
				<u-cell-item :icon=item.icon :title=item.title @click="changepage(index)"></u-cell-item>
			</view>
			
		</u-cell-group>


		<u-cell-group title="骚操作">

			<view class="wrap">
				<u-row gutter="16">
					<u-col span="3" v-for="(item,index) in itemBoxTwo">
						<view class="demo-layout bg-purple">
							<u-button @click="saocclick(index)">{{item.title}}</u-button>
						</view>
					</u-col>
				</u-row>
			</view>
			
		</u-cell-group>
		
		<u-cell-group title="其他">
			
			<view class="wrap">
				<u-row gutter="16">
					<u-col span="3" v-for="item in itemBoxThree">
						<view class="demo-layout bg-purple">
							<u-button>{{item.title}}</u-button>
						</view>
					</u-col>
				</u-row>
			</view>
			
		</u-cell-group>
	</view>
</template>

<script>
	import {
		itemboxExtend
	} from './extend.js'
	export default {
		data() {
			return {
				itemBox: itemboxExtend.itemBox,
				itemBoxTwo: itemboxExtend.itemBoxTwo,
				itemBoxThree:itemboxExtend.itemBoxThree,
				subtitles:'加油,打工人',
			}
		},
		methods:{
			saocclick(idx){
				// console.log(itemboxExtend.itemBoxTwo[idx].url);
				uni.navigateTo({
					url:itemboxExtend.itemBoxTwo[idx].url
					
				})
			},
			changepage(idx){
				uni.navigateTo({
					url:itemboxExtend.itemBox[idx].url
				})
				// console.log(itemboxExtend.itemBox[idx].url);
				// idx==0?uni.navigateTo({
				// 	url:'../topList/translate'
				// }):null;
			},
			nowtime(){
				let data = new Date();
				if(data.getHours()>22 || data.getHours()<5){
					this.subtitles = "晚安,不要熬夜了喔"
				}
			}
		},
		created(){
			this.nowtime();
		}
	}
</script>

<style lang="scss" scoped>
	.wrap {

		padding: 24rpx;
	}

	.u-row {

		margin: 3rpx 0;
	}

	.demo-layout {
		height: 80rpx;
		border-radius: 8rpx;
		margin-top: 10rpx;
	}

	.bg-purple {
		background: #d3dce6;
	}

	.bg-purple-light {
		background: #e5e9f2;
	}

	.bg-purple-dark {
		background: #99a9bf;
	}
	.topbar{
		width: 750rpx;
		height: 150rpx;
		background-color: #19BE6B;
		text-align: left;
		padding-left: 20rpx;
		line-height: 180rpx;
		font-size: 60rpx;
		color: #FFFFFF;
		
		.subtitles{
			font-size: 23rpx;
			margin-left: 30rpx;
		}
		}
</style>
