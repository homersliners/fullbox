<template>
	<view>
		<view style="margin: 10rpx;">
			<u-button :border="btnborder" @click="successbtn">{{btnname}}</u-button>
			<view class="textstyle" v-show="box==true">
				<!-- <strong style="font-size: 36rpx;">{{titles}}</strong> -->
				<!-- <br/> -->
				
				{{names}}
			</view>
			
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				btnname: "生成彩虹屁",
				btnborder: true,
				box:false,
				showText: "我爱时间",
				names: "",
				titles:"",
			}
		},
		methods: {
			successbtn() {
				this.box = true;
				uni.request({
					url: "https://api.muxiaoguo.cn/api/caihongpi",
					success: (res) => {
						// this.titles =res.data.data.title
						this.names = res.data.data.comment
					}
				})

			}
		}
	}
</script>

<style lang="scss" scoped>
	.textstyle {
		width: auto;
		margin-top: 10rpx;
		height: auto;
		background-color: #F8F8F8;
		padding: 15rpx;
		font-size: 28rpx;

	}
</style>
