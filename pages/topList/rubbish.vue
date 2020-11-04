<!-- admin：林轩 -->
<template>
	<view class="boxStyle">
		<view style="margin: 10rpx;">
			<u-input class="boxStyleInput" placeholder="你是什么垃圾" :border='inputborder' v-model="inputdata" />
			<u-button type="success" class="boxStyleButton" @click="find()">{{btnname}}</u-button>
		</view>

		<!-- <u-input v-model="outtext" border="true" class="boxStyleInput" placeholder=""></u-input> -->
		<view class="commitBox" v-show="showDescription==true">
			<u-card title="类型">
				<view class="" slot="body">
					{{inputdatadomo}}---{{Type}} {{Description.Concept}}
				</view>
				<view slot='foot'>
					同类: {{Description.Including}}
				</view>
			</u-card>
			<u-card title="建议">
				<view class="" slot="body">
					{{Description.Release_requirement}}
				</view>
				<!-- <view slot='foot'>
				
				</view> -->
			</u-card>
		</view>
		<u-toast ref="uToast" />
	</view>
</template>

<script>
	export default {

		data() {
			return {
				inputborder: true,
				inputdata: "",
				inputdatadomo: '',
				btnname: '查一下',
				Type: '',
				showDescription: false,
				Description: {
					Concept: '',
					Including: '',
					Release_requirement: '',
				},
			}
		},
		watch: {

		},
		methods: {
			find() {
				if (this.inputdata != "") {
					this.btnname = "加载中";

					uni.request({
						url: "https://api.muxiaoguo.cn/api/lajifl",
						data: {
							m: encodeURIComponent(this.inputdata),
							// m:this.inputdata,
						},
						success: (res) => {
							if (res.data.code == "200") {
								this.inputdatadomo = this.inputdata;
								this.btnname = "查一下";
								this.showDescription = true;
								//显示属性
								this.Type = res.data.data.Type;
								res.data.data.Description.Concept != null ? this.Description.Concept = "(" + res.data.data.Description.Concept +
									")" :
									this.Description.Concept = "";
								//同类
								res.data.data.Description.Including != null ? this.Description.Including = res.data.data.Description.Including :
									this.Description.Including = "无";
								//建议
								res.data.data.Description.Release_requirement != null ? this.Description.Release_requirement = res.data.data
									.Description.Release_requirement : this.Description.Release_requirement = "无";
							} else {
								this.btnname = "查一下";
								this.showDescription = false;
								this.$refs.uToast.show({
									title: '无法识别喔',
									type: 'warn',

								})
							}

						},
						fail: (res) => {
							this.btnname = "加载失败";
						}
					})
				} else {
					this.$refs.uToast.show({
						title: '请输入内容喔',
						type: 'warn',

					})
				}
			}
		}

	}
</script>

<style lang="scss" scoped>
	.boxStyle {

		// margin: 25rpx;
		// width: 700rpx;
		// height: 200rpx;
		// // background-color: #007AFF;
		.boxStyleInput {
			margin: 25rpx;
			width: 700rpx;
			height: 200rpx;
			// background-color: #19BE6B;
		}

		.boxStyleButton {
			margin: 25rpx;
			width: 700rpx;
		}

		// position: fixed;
	}

	.commitBox {
		width: 700rpx;
		height: 400rpx;
		// background-color: #18B566;
		margin: 25rpx;
		border-radius: 20rpx;
	}
</style>
