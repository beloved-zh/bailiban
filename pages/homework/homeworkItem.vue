<template>
	<view>
		<view class="homework-item" v-for="item in homeworkItem" :key="item.id">
			<view class="head">
				<view class="name">{{item.name}}</view> 
				<view class="score">
					
					<text v-if="(item.homeworkLogInfo) != null">
						<text class="iconfont icon-xingxing1" v-for="n in item.homeworkLogInfo.score"></text>
						<text class="iconfont icon-xingxing" v-for="n in (5-(item.homeworkLogInfo.score))"></text>
						<text class="iconfont icon-xingxing" v-if="item.homeworkLogInfo.score==null" v-for="n in 5"></text>
					</text>
					<text class="iconfont icon-xingxing" v-else v-for="n in 5"></text>
				</view>
			</view>
			<view class="main">
				<view class="left">
					<view class="thirdTagName">{{item.thirdTagName}}</view>
					<view class="expireDate">
						<text class="iconfont icon-shizhong"></text>{{item.expireDate}}</view>
				</view>
				<view class="right" style="color: #24b38d;" v-if="(item.homeworkLogInfo) != null">
					已提交
				</view>
				<view class="right" style="color: #666666;" v-if="(item.homeworkLogInfo) == null">
					未提交
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				homeworkItem: [],
				score: 5
			}
		},
		methods: {
			
		},
		onLoad(option) {
			this.classId = option.classId;
			this.createBy = option.createBy;
			this.firstTagId = option.firstTagId;
			this.secondTagId = option.secondTagId;
			
			uni.request({
				url:"https://api.bailiban.com/api/admin/miniprogram/homework/computer/work/list?classId="+option.classId+"&createBy="+option.createBy+"&firstTagId="+option.firstTagId+"&secondTagId="+option.secondTagId,
				header:{
					"Authorization":uni.getStorageSync("blbUI-token")
				},
				success: (res) => {
					console.log(res.data)
					if(res.data.status == 200){
						console.log(res.data.data)
						this.homeworkItem = res.data.data
						/* this.score = res.data.data.homeworkLogInfo.score */
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
	page{
		background-color: #f2f2f2;
	}
	.homework-item{
		width: 700rpx;
		height: 250rpx;
		margin: 25rpx;
		border-radius: 10px;
		background-color: #FFFFFF;
		.head{
			width: 650rpx;
			height: 80rpx;
			line-height: 80rpx;
			margin: 0 auto;
			border-bottom: 1px solid #eee;
			display: flex;
			flex-direction: row;
			justify-content: space-between;
			.name{
				font-size: 28rpx;
				color: #787878;
			}
			.score{
				.icon-xingxing1{
					color: #ffc0cb;
				}
			}
		}
		.main{
			width: 650rpx;
			margin: 0 auto;
			display: flex;
			flex-direction: row;
			justify-content: space-between;
			.left{
				height: 130rpx;
				margin: auto 0;
				display: flex;
				flex-direction: column;
				justify-content: space-between;
				.thirdTagName{
					font-size: 35rpx;
					font-weight: bold;
				}
				.expireDate{
					font-size: 28rpx;
					color: #474747;
				}
			}
			.right{
				line-height: 170rpx;
				font-size: 28rpx;
				/* color: $color; */
			}
		}
	}
</style>
