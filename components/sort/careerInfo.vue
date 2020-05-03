<template>
	<view class="projectInfo">
		<view class="head">
			<view class="projectName">{{info.careerPlanning.careerplanningName}}</view>
			<view>
				<button size="mini" @click="jionProject">加入项目</button>
			</view>
		</view>
		<view class="info">
			<view class="personAllow">学习人数<text>\n</text>{{info.expectedLearningNumber}}</view>
			<view class="projectPrice">课程<text>\n</text>{{info.careerPlanning.coursenumber}}</view>
			<view class="projectDays">学习周期<text>\n</text>{{info.careerPlanning.expectedlearningtime}}个月</view>
		</view>
		<rich-text :nodes="info.careerPlanning.content" space="nbsp"></rich-text>
	</view>
</template>

<script>
	export default {
		props:["cid"],
		data() {
			return {
				info: []
			}
		},
		methods: {
			getInfo(){
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/careerplanning/info/"+this.cid,
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					method:"GET",
					dataType:"json",
					success:(res)=> {
						console.log(res)
						if(res.data.status == 200){
							this.info = res.data.data
						}else{
							uni.showToast({
								title:'请求数据失败'
							})
						}
					},
					fail() {
						uni.showToast({
							title:'请求接口失败'
						})
					}
				});
			},
			jionProject(){
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/my/jionCareerplanning/"+this.cid,
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					method:"POST",
					dataType:"json",
					success:(res)=> {
						console.log(res)
						if(res.data.status == 200){
							uni.showModal({
								content: "加入成功",
								showCancel: false,
								success: function (res) {
									if (res.confirm) {
										console.log('用户点击确认');
									} else if (res.cancel) {
										console.log('用户点击取消');
									}
								}
							})
						}else{
							uni.showModal({
								content: res.data.message,
								showCancel: false,
								success: function (res) {
									if (res.confirm) {
										console.log('用户点击确认');
									} else if (res.cancel) {
										console.log('用户点击取消');
									}
								}
							})
						}
					},
					fail() {
						uni.showToast({
							title:'请求接口失败'
						})
					}
				});
			}
		},
		//组件调用后
		created(){
			this.getInfo()
		}
	}
</script>

<style lang="scss">
	.projectInfo{
		width: 700rpx;
		margin: 25rpx;
		.head{
			width: 700rpx;
			display: flex;
			flex-direction: row;
			justify-content: space-between;
			.projectName{
				line-height: 70rpx;
			}
			button{
				width: 150rpx;
				height: 70rpx;
				font-size: 23rpx;
				line-height: 70rpx;
				text-align: center;
				color: #FFFFFF;
				background-color: #07c160;
				border-radius: 50rpx;
			}
		}
		.name{
			width: 700rpx;
			margin-top: 20rpx;
			display: flex;
			flex-direction: row;
			.managerName{
				width: 50%;
			}
			.teacherName{
				width: 50%;
			}
		}
		.info{
			margin: 25rpx 0 30rpx 0;
			width: 700rpx;
			display: flex;
			flex-direction: row;
			justify-content: space-between;
			color: #848484;
			text-align: center;
		}
		rich-text{
			color: #848484;
			line-height: 60rpx;
			font-size: 28rpx;
		}
	}
	

</style>
