<template>
	<view class="page">
		<view class="project" v-for="(item,index) in projects" :key="index" @click="gotProject(item.id)">
			<view class="left" :style="colors[index]">
				<text v-if="item.projectName.length < 20">{{item.projectName}}</text>
				<text v-else>{{item.projectName.substring(0,17)}}...</text>
			</view>
			<view class="right">
				任务提交{{projectProgress[index].checkStateNum}}/{{projectProgress[index].planNum}}
			</view>
		</view>
		<view class="falg" v-if="falg">亲，已经到底了哦~</view>
	</view>
</template>

<script>
	export default {
		props:["pageNum"],
		data() {
			return {
				projects: [],
				projectProgress: [],
				falg: false,
				colors: ["background-color:#5ec9af","background-color:#f7c868","background-color:#7dafef","background-color:#f07783","background-color:#72ccec","background-color:#fb998e","background-color:#e18cde","background-color:#e28ca8"]
			}
		},
		methods: {
			getProject(){
				//我的项目
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/my/project?pageSize=8&pageNum="+this.pageNum,
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					method:"GET",
					dataType:"json",
					success:(res)=> {
						if(res.data.status == 200){
							if(res.data.data.rows.length < 8){
								this.falg = true
							}
							this.colors = [...this.colors,...this.colors]
							this.projects = [...this.projects,...res.data.data.rows];
							for(let row of res.data.data.rows){
								this.getProjectProgress(row.id)
							}
						}else{
							uni.showToast({
								title:'请求接口失败'
							})
						}
					}
				});
			},
			//获取提交数据
			getProjectProgress(id){
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/my/projectProgress/"+id,
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					method:"GET",
					dataType:"json",
					success:(res)=> {
						if(res.data.status == 200){
							/* this.projectProgress = [...this.projectProgress,...res.data.data]; */
							this.projectProgress.push(res.data.data)
						}else{
							uni.showToast({
								title:'请求接口失败'
							})
						}
					}
				});
			},
			gotProject(id){
				uni.navigateTo({
					url:"../../pages/project/project?id="+id
				});
			}
		},
		//组件调用后
		created(){
			this.getProject()
		}
	}
</script>

<style lang="scss">
	.page{
		background-color: #f2f2f2;
	}
	.project{
		width: 750rpx;
		height: 150rpx;
		margin-top: 10rpx;
		background-color: #FFFFFF;
		display: flex;
		flex-direction: row;
		.left{
			width: 300rpx;
			height: 150rpx;
			color: #FFFFFF;
			border-radius: 7rpx;
			text-align: center;
			display: flex;
			flex-direction: column;
			justify-content: center;
		}
		.right{
			margin-left: 20rpx;
			line-height: 150rpx;
			letter-spacing: 10rpx;
		}
	}
	.falg{
		width: 750rpx;
		height: 80rpx;
		background-color: #f2f2f2;
		color: $color;
		font-size: 20rpx;
		text-align: center;
		line-height: 80rpx;
		margin-top: 20rpx;
	}
</style>
