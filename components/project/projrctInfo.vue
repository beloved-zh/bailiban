<template>
	<view class="projectInfo">
		<view class="head">
			<view class="projectName">{{info.project.projectName}}</view>
			<view><button size="mini" @click="jionProject">加入项目</button></view>
		</view>
		<view class="name">
			<view class="managerName">项目经理：{{info.project.managerName}}</view>
			<view class="teacherName">项目督导：{{info.project.teacherName}}</view>
		</view>
		<view class="info">
			<view class="personAllow">学习人数<text>\n</text>{{info.project.personAllow}}</view>
			<view class="projectPrice">费用<text>\n</text>￥
				{{info.project.projectPrice == null ? '0' : info.project.projectPrice}}
			</view>
			<view class="projectDays">任务周期<text>\n</text>{{info.project.projectDays}}天</view>
			<view class="projectType">类型<text>\n</text>{{info.project.projectType==null?"":info.project.projectType}}</view>
		</view>
		<rich-text :nodes="info.project.projectContext" space="nbsp"></rich-text>
	</view>
</template>

<script>
	export default {
		props:["pid"],
		data() {
			return {
				info: []
			}
		},
		methods: {
			getInfo(){
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/project/info?id="+this.pid,
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
							uni.showModal({
								content: res.data.message,
								showCancel: false,
								success: function (res) {
									if (res.confirm) {
										uni.navigateBack({
											delta: 1
										});
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
			},
			jionProject(){
				if(this.info.project.projectPrice != 0){
					uni.showModal({
						title:"确认购买",
						content:"本项目为收费项目，价格为"+this.info.project.projectPrice+",是否需要购买学习",
						success: function (res) {
							if (res.confirm) {
								uni.showToast({
									title:'不支持购买'
								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				}else{
					uni.request({
						url:"https://api.bailiban.com/api/admin/miniprogram/my/jionProject/"+this.pid,
						header:{
							"Authorization":uni.getStorageSync("blbUI-token")
						},
						method:"POST",
						dataType:"json",
						success:(res)=> {
							console.log(res)
							if(res.data.status == 200){
								
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
