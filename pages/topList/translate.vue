<!-- admin：林轩 -->
<template>
	<view>
		<!-- 顶部选择下拉框 -->
		<view class="wrap">
			<u-row gutter="16">
				<u-col span="4">
					<!-- <view> -->
						<!-- <u-input v-model="tratypeLeft" type="select" @click="traleft = true" placeholder="自动" /> -->
						<!-- <u-action-sheet :list="actionSheetList" v-model="traleft" @click="startlanguage"></u-action-sheet> -->
					<!-- </view> -->
				</u-col>
				<u-col span="4"></u-col>
				<u-col span="4">
					<view>
						<u-input v-model="tratypeRight" type="select" @click="traright = true" placeholder="默认(English)" />
						<u-action-sheet :list="actionSheetList" v-model="traright" @click="endlanguage"></u-action-sheet>
					</view>
				</u-col>
			</u-row>
		</view>

		<!-- 翻译输入内容 -->
		<view class="inputbox">
			<u-input placeholder="请输入翻译的内容" v-model="inputText" :border="inputborder" type="textarea" class="inputText"></u-input>
		</view>
		<view style="margin: 10rpx;">
			<!-- 翻译按键 -->
			<u-button type="success" class="tranbtn" @click="translate()">{{btnname}}</u-button>
		</view>
		<view class="outputbox">
			<u-input placeholder="翻译的结果" v-model="outputText" :border="inputborder" type="textarea" class="inputText" :disabled="disabledtype"></u-input>
		</view>
		<u-toast ref="uToast" />
	</view>
</template>

<script>
	export default {
		data() {
			return {
				inputborder:true,
				disabledtype:true,
				btnname: '翻译',
				outputText: '',
				inputText: '',
				tratypeLeft: "",
				tratypeLeftIndex: '',
				tratypeRight: '',
				tratypeRightIndex: 1,
				traleft: false,
				traright: false,
				actionSheetList: [{
						text: '简体中文',
						suo: 'zh-CN',
					},
					{
						text: 'English',
						suo: 'en',
					},

				],
			}
		},
		methods: {
			//选择输入语言
			startlanguage(index) {
				this.tratypeLeftIndex = index;
				this.tratypeLeft = this.actionSheetList[index].text;
			},
			endlanguage(index) {
				this.tratypeRightIndex = index;
				this.tratypeRight = this.actionSheetList[index].text;
			},
			//启动翻译
			translate() {
				if (this.inputText != "") {
					// console.log("success");
					this.translateOk();
					// this.aaa();
				} else {
					this.$refs.uToast.show({
						title: '请输入内容喔',
						type: 'warn',

					})
				}

			},
			// aaa(){
			// 	console.log("res");
			// },
			translateOk() {
				this.btnname = "加载中";
				let uplangle;
				let inlangle;
				//必选
				if (this.tratypeRightIndex == 1) {
					uplangle = 'en'
					// console.log("-----",this.tratypeRightIndex);
				} else {
					// console.log(uplangle);
					// uplangle = this.actionSheetList[this.tratypeRightIndex].suo
					uplangle = 'zh-CN'
				}
				//可选
				// if (this.tratypeLeftIndex === 0) {
				// 	// console.log(22);
				// 	inlangle = 'zh-CN'
				// } else if (this.tratypeLeftIndex == '') {
				// 	// inlangle = 
				// } else {
				// 	inlangle = this.actionSheetList[this.tratypeLeftIndex].suo
				// }

				console.log(inlangle, uplangle);
				uni.request({
					url: 'https://api.muxiaoguo.cn/api/Tn_google',
					data: {
						word: this.inputText,
						se: uplangle,
						},

					success: (res) => {
						// console.log(res);
						this.btnname = "翻译";
						this.outputText = res.data.data.Translation;
						
					},
					fail: (res) => {
						// console.log('fail');
						this.btnname = "翻译";
						this.outputText = "翻译失败";
					},
					complete: (res) => {
						// console.log('complete');
					}
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.tranbtn {
		width: 720rpx;
		align-items: center;

	}

	.inputbox {
		width: 730rpx;
		// background-color: #909399;
		// height: 200rpx;
		margin: 10rpx;

		.inputText {
			height: 200rpx;
		}
	}

	.outputbox {
		width: 730rpx;
		// background-color: #909399;
		height: 200rpx;
		margin: 10rpx;

		.inputText {
			height: 200rpx;
		}
	}

	.topbox {
		width: 750rpx;
		height: auto;
		background-color: #333333;

		.boxleft {
			// margin-left: 20rpx;
			width: 300rpx;
			float: left;
			background-color: #007AFF;

		}

		.boxright {
			background-color: #18B566;
			// position: fixed;

			width: 70rpx;
			// float: left;
		}
	}
</style>
