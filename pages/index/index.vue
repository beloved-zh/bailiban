<template>
	<view class="content">
		<!-- 轮播图 -->
		<swiper indicator-dots autoplay circular interval="1000" indicator-color="#ffffff" indicator-active-color="#12c494">
			<swiper-item v-for="item in swipers" :key="item.id">
				<image mode="aspectFill" :src="item.imageDisplay"></image>
			</swiper-item>
		</swiper>
		<!-- 导航 -->
		<view class="nav">
			<view class="nav_time" @click="navItemClick(item.path)" v-for="(item,index) in nav" :key="index">
				<view :class="item.icon"></view>
				<text>{{item.title}}</text>
			</view>
		</view> 
		<!-- 作业 -->
		<view class="main">
			<!-- 没有登陆 -->
			<navigator class="noLogin" url="../login/login" hover-class="none" v-if="flag">
				<image mode="aspectFit" src="../../static/icon/book.png"></image>
				<view>登录后查看练习记录</view>
			</navigator>
			<!-- 登录之后 -->
			<view v-else="flag">
				<view class="exercise">
					<view class="head">最近错题</view>
					<view class="exercise-row">
						<view class="exercise-item">
							<view class="type">练习题库</view>
							<view v-if="exercise.length >= 1">
								<view class="parentName">{{exercise[0].parentName}}</view>
								<view class="name">{{exercise[0].name}}</view>
								<view class="questionNum">共{{exercise[0].questionNum}}道题</view>
							</view>
							<view class="not" v-else>
								暂无题目
							</view>
							<image mode="aspectFit" src="../../static/icon/zjct01.png"></image>
						</view>
						<view class="exercise-item">
							<view class="type">面试题库</view>
							<view v-if="exercise.length >= 2">
								<view class="parentName">{{exercise[1].parentName}}</view>
								<view class="name">{{exercise[1].name}}</view>
								<view class="questionNum">共{{exercise[1].questionNum}}道题</view>
							</view>
							<view class="not" v-else>
								暂无题目
							</view>
							<image mode="aspectFit" src="../../static/icon/zjct02.png"></image>
						</view>
					</view>
					<view class="head">最近收藏</view>
					<view class="exercise-row">
						<view class="exercise-item">
							<view class="type">练习题库</view>
							<view v-if="exercise.length >= 3">
								<view class="parentName">{{exercise[2].parentName}}</view>
								<view class="name">{{exercise[2].name}}</view>
								<view class="questionNum">共{{exercise[2].questionNum}}道题</view>
							</view>
							<view class="not" v-else>
								暂无题目
							</view>
							<image mode="aspectFit" src="../../static/icon/zjsc01.png"></image>
						</view>
						<view class="exercise-item">
							<view class="type">面试题库</view>
							<view v-if="exercise.length >= 4">
								<view class="parentName">{{exercise[3].parentName}}</view>
								<view class="name">{{exercise[3].name}}</view>
								<view class="questionNum">共{{exercise[3].questionNum}}道题</view>
							</view>
							<view class="not" v-else>
								暂无题目
							</view>
							<image mode="aspectFit" src="../../static/icon/zjsc02.png"></image>
						</view>
					</view>
				</view>
				<view class="work">
					<view class="head">
						<view>我的作业</view>
						<view>
						<navigator url="../homework/homework" hover-class="none">更多></navigator>
						</view>
					</view>
					<view class="work-time" v-for="(item,index) in homeworks" v-if="(index < 2)" :key="item.id">
						<view class="type">上机作业</view>
						<view class="work-main">
							<view class="left">
								<view class="work-time-name">{{item.firstTagName}}-{{item.secondTagName}}</view>
								<view class="work-time-sum">
									<text>任务:{{item.workSum}}</text>
									<text>提交:{{item.submitSum}}</text>
								</view>
							</view>
							<view class="right">
								<button @click="goHomeworkItem(item.classId,item.createBy,item.firstTagId,item.secondTagId)">去查看</button>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				swipers:[],
				nav:[
					{
						icon:"iconfont icon-dingshirenwu",
						title:"每日一册",
						path:"../daily/daily"
					},
					{
						icon:"iconfont icon-renwuguanli",
						title:"我的作业",
						path:"../homework/homework"
					},
					{
						icon:"iconfont icon-miaozhun-",
						title:"练习题库",
						path:""
					},
					{
						icon:"iconfont icon-renwu",
						title:"面试题库",
						path:""
					}
				],
				flag : true,
				homeworks: [],
				exercise: []
			}
		},
		onLoad() {
			this.getSwipers();
		},
		methods: {
			//获取轮播图
			getSwipers(){
				uni.request({
					url:"https://api.bailiban.com/api/admin/open/v1/api/banner",
					success: (res) => {
						if(res.data.status == 200){
							this.swipers = res.data.data
						}else{
							uni.showToast({
								title:'请求接口失败'
							})
						}
					}
				})
			},
			//导航点击处理函数
			navItemClick(url){
				if(this.flag){
					uni.showModal({
						content:"请登录后进行操作",
						confirmColor:"#3485fb",
						showCancel:false
					});
					return false;
				}
				uni.navigateTo({
					url:url
				})
			},
			//获取作业
			getHomework(){
				uni.request({
					url:"https://api.bailiban.com/api/admin/apis/user/homework?type=1&page=1&limit=10",
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					success: (res) => {
						if(res.data.status == 200){
							this.homeworks = res.data.data.rows
						}else{
							uni.showToast({
								title:'请求接口失败'
							})
						}
					}
				})
			},
			//获取练习信息
			getExercise(){
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/home/exercise",
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					success: (res) => {
						if(res.data.status == 200){
							this.exercise = res.data.data
						}else{
							uni.showToast({
								title:'请求接口失败'
							})
						}
					}
				})
			},
			goHomeworkItem(classId,createBy,firstTagId,secondTagId){
				uni.navigateTo({
					url: "../homework/homeworkItem?classId="+classId+"&createBy="+createBy+"&firstTagId="+firstTagId+"&secondTagId="+secondTagId
				})
			}
		},
		onShow() {
			if(uni.getStorageSync("blbUI-token") == ""){
				this.flag = true
			}else{
				this.flag = false
				this.getHomework()
				this.getExercise()
			}
		}
	}
</script>

<style lang="scss">
	page{
		background-color: #f2f2f2;
	}
	swiper{
		height: 340rpx;
		image{
			width: 100%;
			height: 100%;
		}
	}
	
	.nav{
		background-color: #FFFFFF;
		height: 200rpx;
		display: flex;
		padding-bottom: 30rpx;
		.nav_time{
			width: 25%;
			text-align: center;
			.iconfont{
				width: 110rpx;
				height: 110rpx;
				background-color: $color;
				//弧度
				border-radius: 50%;
				margin: 30rpx auto 20rpx auto;
				//行高
				line-height: 110rpx;
				color: #ffF;
				font-size: 60rpx;
			}
			text{
				font-size: 30rpx;
				color: #5f5f5f;
			}
		}
	}
	
	.main{
		width: 100%;
		.noLogin{
			margin: 100rpx auto;
			text-align: center;
			image{
				width: 300rpx;
				height: 300rpx;
			}
			view{
				color: $color;
			}
		}
		.exercise{
			width: 700rpx;
			margin: 0 25rpx;
			.exercise-row{
				width: 700rpx;
				display: flex;
				flex-direction: row;
				justify-content: space-between;
				.exercise-item{
					width: 340rpx;
					height: 500rpx;
					background-color: #FFFFFF;
					border-radius: 7px;
					text-align: center;
					.parentName{
						font-size: 38rpx;
						line-height: 100rpx;
					}
					.name{
						font-size: 32rpx;
						color: #666666;
					}
					.questionNum{
						font-size: 28rpx;
						color: #a2a2a2;
						line-height: 60rpx;
					}
					image{
						width: 250rpx;
						height: 250rpx;
					}
					.not{
						font-size: 32rpx;
						color: #666666;
						line-height: 200rpx;
					}
				}
			}
		}
		.work{
			width: 700rpx;
			margin: 0 25rpx;
			.work-time{
				background-color: #FFFFFF;
				width: 100%;
				height: 300rpx;
				border-radius: 7px;
				margin-bottom: 20rpx;
				.work-main{
					width: 640rpx;
					height: 190rpx;
					margin: 30rpx;
					display: flex;
					flex-direction: row;
					justify-content: space-between;
					.left{
						display: flex;
						flex-direction: column;
						justify-content: space-between;
						.work-time-name{
							font-size: 32rpx;
							font-weight: bold;
						}
						.work-time-sum{
							font-size: 28rpx; 
							text:nth-child(1){
								color: #666666;
							}
							text:nth-child(2){
								margin-left: 50rpx;
								color: $color;
							}
						}
					}
					.right{
						margin: auto 0;
						button{
							color: #FFFFFF;
							background-color: $color;
							border-radius: 30px;
							width: 200rpx;
							height: 80rpx;
							font-size: 28rpx;
							line-height: 80rpx;
						}
					} 
				}
			}
		}
	}
	.head{
		width: 100%;
		height: 100rpx;
		line-height: 100rpx;
		display: flex;
		view:nth-child(1){
			width: 350rpx;
			text-align: left;
		}
		view:nth-child(2){
			width: 350rpx;
			text-align: right;
			color: #666666;
			font-size: 28rpx;
		}
	}
	.type{
		width: 150rpx;
		height: 50rpx;
		background-color: #ff6e2e;
		border-radius: 15rpx 0;
		color: #FFFFFF;
		font-size: 25rpx;
		line-height: 50rpx;
		text-align: center;
	}
	
</style>
