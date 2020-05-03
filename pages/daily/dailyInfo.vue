<template>
	<view>
		<view class="head">
			<view class="head-main">
				<view class="pmName"><view>项目经理：</view><view>{{pmName}}</view></view>
				<view class="tagName"><view>测试内容：</view><view>{{tagName}}</view></view>
			</view>
		</view>
		<view class="main">
			<view class="dailInfo-item" v-for="row in rows" :key="row.id">
				<view class="dailInfo-head">
					<text class="title">
						<text class="type" v-if="row.type == 0">单选题</text>
						<text class="type" v-else>多选题</text>
						{{row.title}}
					</text>
				</view>
				<view class="optionsList">
					<view class="options" v-for="item in row.optionsList" :key="item.id">
						<view class="options-time" v-if="(item.sort == 1)&&(item.isAnswer == item.isUserAnswer)&&(item.isAnswer == false)">A</view>
						<view class="options-time" v-else-if="(item.sort == 1)&&(item.isAnswer != item.isUserAnswer)&&(item.isAnswer == true)" style="background-color: #12c494;border: 0;">A</view>
						<view class="options-time" v-else-if="(item.sort == 1)&&(item.isAnswer != item.isUserAnswer)&&(item.isAnswer == false)" style="background-color: #ff0000;border: 0;">
							<text class="iconfont icon-cuohao"></text>
						</view>
						<view class="options-time" v-else-if="(item.sort == 1)&&(item.isAnswer == item.isUserAnswer)&&(item.isAnswer == true)" style="background-color: #12c494;border: 0;">
							<text class="iconfont icon-duihao"></text>
						</view>
						
						<view class="options-time" v-else-if="(item.sort == 2)&&(item.isAnswer == item.isUserAnswer)&&(item.isAnswer == false)">B</view>
						<view class="options-time" v-else-if="(item.sort == 2)&&(item.isAnswer != item.isUserAnswer)&&(item.isAnswer == true)" style="background-color: #12c494;border: 0;">B</view>
						<view class="options-time" v-else-if="(item.sort == 2)&&(item.isAnswer != item.isUserAnswer)&&(item.isAnswer == false)" style="background-color: #ff0000;border: 0;">
							<text class="iconfont icon-cuohao"></text>
						</view>
						<view class="options-time" v-else-if="(item.sort == 2)&&(item.isAnswer == item.isUserAnswer)&&(item.isAnswer == true)" style="background-color: #12c494;border: 0;">
							<text class="iconfont icon-duihao"></text>
						</view>
						
						<view class="options-time" v-else-if="(item.sort == 3)&&(item.isAnswer == item.isUserAnswer)&&(item.isAnswer == false)">C</view>
						<view class="options-time" v-else-if="(item.sort == 3)&&(item.isAnswer != item.isUserAnswer)&&(item.isAnswer == true)" style="background-color: #12c494;border: 0;">C</view>
						<view class="options-time" v-else-if="(item.sort == 3)&&(item.isAnswer != item.isUserAnswer)&&(item.isAnswer == false)" style="background-color: #ff0000;border: 0;">
							<text class="iconfont icon-cuohao"></text>
						</view>
						<view class="options-time" v-else-if="(item.sort == 3)&&(item.isAnswer == item.isUserAnswer)&&(item.isAnswer == true)" style="background-color: #12c494;border: 0;">
							<text class="iconfont icon-duihao"></text>
						</view>
						
						<view class="options-time" v-else-if="(item.sort == 4)&&(item.isAnswer == item.isUserAnswer)&&(item.isAnswer == false)">D</view>
						<view class="options-time" v-else-if="(item.sort == 4)&&(item.isAnswer != item.isUserAnswer)&&(item.isAnswer == true)" style="background-color: #12c494;border: 0;">D</view>
						<view class="options-time" v-else-if="(item.sort == 4)&&(item.isAnswer != item.isUserAnswer)&&(item.isAnswer == false)" style="background-color: #ff0000;border: 0;">
							<text class="iconfont icon-cuohao"></text>
						</view>
						<view class="options-time" v-else-if="(item.sort == 4)&&(item.isAnswer == item.isUserAnswer)&&(item.isAnswer == true)" style="background-color: #12c494;border: 0;">
							<text class="iconfont icon-duihao"></text>
						</view>
						
						
						<view class="options-main" v-if="(item.isUserAnswer == false)">{{item.optionContent}}</view>
						<view class="options-main" v-else-if="(item.isUserAnswer == true)&&(item.isAnswer != true)" style="color: #ff0000;">{item.optionContent}}</view>
						<view class="options-main" v-else-if="(item.isUserAnswer == true)&&(item.isAnswer == true)" style="color: #12c494;">{{item.optionContent}}</view>
					</view>
				</view>
				<view class="answer">
					<view class="userAnswer">考生答案：{{row.userAnswer}}</view>
					<view class="rightAnswer">正确答案：{{row.rightAnswer}}</view>
				</view>
				<view class="mydesc">
					<text>答案解析：</text>
					{{row.mydesc}}
				</view>
				<view class="fg"></view>
			</view>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				pmName: "",
				tagName: "",
				rows:[]
			}
		},
		methods: {
			
		},
		onLoad(option) {
			this.pmName = option.pmName;
			this.tagName = option.tagName;
			
			let classId = option.classId;
			let userId = option.userId;
			let weekDate = option.weekDate;
			console.log(classId,userId,weekDate)
			
			uni.request({
				url:"https://api.bailiban.com/api/admin/miniprogram/exam/daily/survey/get?classId="+classId+"&userId="+userId+"&weekDate="+weekDate,
				header:{
					"Authorization":uni.getStorageSync("blbUI-token")
				},
				success: (res) => {
					console.log(res.data)
					if(res.data.status == 200){
						this.rows = res.data.data.rows
					}else{
						uni.showToast({
							title:'请求接口失败'
						})
					}
				}
			})
		}
	}
</script>

<style lang="scss">
	
	.head{
		width: 750rpx;
		background-color: #f2f2f2;
		color: $color;
		.head-main{
			padding: 20rpx 25rpx;
			font-size: 32rpx;
			line-height: 60rpx;
			.pmName,.tagName{
				display: flex;
				flex-direction: row;
				view:nth-child(1){
					display: inline-block;
					white-space: nowrap;
				}
			}
		}
		
	}

	.main{
		.dailInfo-item{
			width: 700rpx;
			.dailInfo-head{
				margin: 25rpx;
				.title{
					width: 100%;
					font-size: 32rpx;
					font-weight: bold;
					line-height: 45rpx;
					.type{
						background-color: $color;
						border-radius: 5px;
						font-size: 26rpx;
						line-height: 40rpx;
						text-align: center;
						color: #FFFFFF;
						padding: 5rpx 10rpx;
						margin-right: 10rpx;
					}
				}
				
			}
			.optionsList{
				margin: 25rpx;
				width: 700rpx;
				.options{
					width: 700rpx;
					display: flex;
					flex-direction: row;
					margin: 25rpx 0;
					.options-time{
						width: 65rpx;
						height: 65rpx;
						border: 1rpx solid #333333;
						border-radius:50%;
						/* background-color: $color; */
						text-align: center;
						line-height: 65rpx;
						.iconfont{
							font-size: 45rpx;
							line-height: 70rpx;
							color: #FFFFFF;
						}
					}
					.options-main{
						width: 610rpx;
						margin-left: 25rpx;
						line-height: 65rpx;
					}
				}
			}
			.answer{
				margin: 25rpx;
				width: 700rpx;
				height: 180rpx;
				border-radius: 10rpx;
				background-color: #f2f2f2;
				// padding: 25rpx;
				display: flex;
				flex-direction: column;
				justify-content: space-between;
				font-size: 32rpx;
				font-weight: bold;
				line-height: 45rpx;
				.userAnswer{
					margin: 25rpx;
				}
				.rightAnswer{
					margin: 25rpx;
				}
			}
			.mydesc{
				margin: 25rpx;
				width: 700rpx;
				line-height: 45rpx;
				text{
					font-size: 32rpx;
					font-weight: bold;
				}
			}
			.fg{
				width: 750rpx;
				height: 10rpx;
				background-color: #f2f2f2;
			}
		}
	}
</style>
