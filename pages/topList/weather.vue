<template>
	<view class="topbox">
		<u-input v-model="cityinput" placeholder="请选择城市(仅限国内)" :border="inputborder" style="margin: 10rpx;"></u-input>
		<u-button style="margin: 10rpx;" type="success" @click="success()">{{btnname}}</u-button>
		<view>
			<!-- <u-image width="750rpx" height="300rpx"></u-image> -->
			<view class="wrap">
				<u-row gutter="16">

					<u-col span="4" class="otherBox" v-for="item in boxs">
						{{item.name}}<br>
						<text class="smallText" style=" font-size: 35rpx;">{{item.data}}</text>
					</u-col>
				</u-row>
			</view>
		</view>
		<u-toast ref="uToast" />
	</view>
</template>

<script>
	export default {
		data() {
			return {
				inputborder:true,
				btnname: '确定',
				cityinput: '',
				boxs: [{
						name: "天气",
						data: "#",
						id: "weather",
					},

					{
						name: "温度",
						data: "#",
						id: "temp",
					},
					{
						name: "风向",
						data: "#",
						id: "WD",
					},
					{
						name: "风速",
						data: "#",
						id: "WS",
					}, {
						name: "湿度",
						data: "#",
						id: "SD",
					}, {
						name: "PM2.5",
						data: "#",
						id: "pm25"
					}
				]
			}
		},
		methods: {
			success() {
				
				if (this.cityinput != "") {
					this.btnname ="加载中";
					uni.request({
						url: "https://api.muxiaoguo.cn/api/tianqi",
						data: {
							city: this.cityinput,
							type: 1
						},
						success: (res) => {
							if(res.data.code=="200"){
								this.btnname ="确认";
								let successbox = res.data.data;
								this.boxs[0].data = successbox.weather;
								this.boxs[1].data = successbox.temp;
								this.boxs[2].data = successbox.WD;
								this.boxs[3].data = successbox.WS;
								this.boxs[4].data = successbox.SD;
								this.boxs[5].data = successbox.pm25;
							}else{
								this.btnname ="确认";
								this.$refs.uToast.show({
									title: '这里是外太空吧',
									type: 'warn',
								
								})
								this.boxs[0].data = "#";
								this.boxs[1].data = "#";
								this.boxs[2].data = "#";
								this.boxs[3].data = "#";
								this.boxs[4].data = "#";
								this.boxs[5].data = "#";
							}
							
							// this.boxs[6].data = successbox.pm25;
						},
						fail: (res) => {
							this.btnname ="确认";
							this.$refs.uToast.show({
								title: '接口获取失败',
								type: 'warn',
							
							})
						}
					})
				} else {
					this.btnname ="确认";
					this.$refs.uToast.show({
						title: '请输入内容喔',
						type: 'warn',

					})
				}

			}
		}
	}
</script>

<style>
	.otherBox {
		/* background-color: #007AFF; */
		height: 140rpx;
		font-size: 25rpx;
		line-height: 50rpx;
		/* margin-left: 30rpx; */
		text-align: center;

	}

	.wrap {
		padding: 24rpx;
		background-color: #f1f1f1;
		border-radius: 20rpx;
		margin: 10rpx;
	}

	.u-row {
		/* margin: 40rpx 0; */
	}
</style>
