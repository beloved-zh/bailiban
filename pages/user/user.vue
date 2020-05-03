<template>
	<view>
		<view class="head">
			<view class="photo">
				<image :src="userInfo.photo"></image>
			</view>
			<view class="info">
				<view class="username">{{userInfo.username}}</view>
				<view class="classNames">{{userInfo.classNames}}</view>
			</view>
			<view class="addClass">
				+加入班级
			</view>
		</view>
		<view class="tab">
			<qs-tabs :tabs="tabs" :current="current" animationLineWidth="80" minWidth="170" activeColor="#ef5656" @change="change"></qs-tabs>
		</view>
		<view class="main">
			<daily v-if="(current == 0)&&(userInfo != null)" :pageNum="pageNum" ref="daily"></daily>
			<vocation v-if="current == 1" :pageNum="pageNum" ref="vocation"></vocation>
			<project v-if="current == 2" :pageNum="pageNum" ref="project"></project>
			<course v-if="current == 3" :pageNum="pageNum" ref="course"></course>
			<exam v-if="current == 4"></exam>
		</view>
	</view>
</template>

<script>
	import QsTabs from '@/components/QS-tabs/QS-tabs.vue';
	import daily from '@/components/daily/daily.vue';
	import vocation from '@/components/vocation/vocation.vue';
	import project from '@/components/project/project.vue';
	import exam from '@/components/exam/exam.vue';
	import course from '@/components/course/course.vue';
	export default {
		data() {
			return {
				userInfo:{},
				tabs:[
					{name:"每日一册"},
					{name:"我的职业"},
					{name:"我的项目"},
					{name:"我的课程"},
					{name:"我的考试"}
				],
				current: 0,
				pageNum: 1
			}
		},
		methods: {
			change(index){
				this.pageNum = 1;
				this.current = index;
			},
			
			//判断是否登录
			judgeLogin(){
				let token = uni.getStorageSync("blbUI-token");
				if(token == ""){
					uni.showModal({
						title:"提示",
						content:"请登录后查看",
						confirmColor:"#3485fb",
						success(res) {
							if (res.confirm) {
								uni.navigateTo({
									url:"../login/login"
								})
							} else if (res.cancel) {
								uni.switchTab({
									url:"../index/index"
								})
							}
							
						}
					});
				}else{
					//获取用户信息
					uni.request({
						url:"https://api.bailiban.com/api/admin/miniprogram/my/info",
						header:{
							"Authorization":uni.getStorageSync("blbUI-token")
						},
						method:"GET",
						dataType:"json",
						success:(res)=> {
							if(res.data.status == 200){
								this.userInfo = res.data.data
								uni.setStorageSync("userInfo",res.data.data);
							}else{
								uni.showModal({
									content:res.data.message,
									showCancel:false,
								});
							}
						}
						
					});
				}
			}
		},
		onLoad() {
		},
		onShow() {  //页面显示
			this.judgeLogin()
		},
		components: {
			QsTabs,
			daily,
			vocation,
			project,
			exam,
			course
		},
		onReachBottom() {
			console.log("页面触底，查询下一页数据")
			this.pageNum++;
			
			if(this.current == 0){
				this.$refs.daily.getDailys()
			}else if(this.current == 1){
				this.$refs.vocation.getInfo()
			}else if(this.current == 2){
				this.$refs.project.getProject()
			}else if(this.current == 3){
				this.$refs.course.getCoures()
			}
		}
	}
</script>

<style lang="scss">
	.head{
		height: 200rpx;
		width: 710rpx;
		margin: 20rpx;
		display: flex;
		flex-direction: row;
		.photo{
			width: 200rpx;
			height: 200rpx;
			image{
				width: 100%;
				height: 100%;
			}
		}
		.info{
			height: 150rpx;
			margin: 25rpx;
			font-size: 35rpx;
			display: flex;
			flex-direction: column;
			justify-content: space-between;
		}
		.addClass{
			margin: 25rpx;
			color: $color;
			font-size: 32rpx;
		}
	}
	.tab{
		width: 750rpx;
		border: 1rpx solid #eee;
	}
	/* .main{
		width: 700rpx;
		margin: auto;
	} */
</style>
