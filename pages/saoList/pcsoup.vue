<template>
	<view>
		<view style="margin: 10rpx;">
			<u-button :border="btnborder" @click="successbtn">{{btnname}}</u-button>
			<view class="textstyle" v-show="box == true">
				{{names}}
			</view>
			
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				btnname: "随机生成毒鸡汤",
				btnborder: true,
				box:false,
				showText: "我爱时间",
				names: "",
			}
		},
		methods: {
			successbtn() {
				this.box = true;
				uni.request({
					url: "https://api.muxiaoguo.cn/api/dujitang",
					success: (res) => {
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
		height: 200rpx;
		background-color: #F8F8F8;
		padding: 15rpx;
		font-size: 28rpx;

	}
</style>
