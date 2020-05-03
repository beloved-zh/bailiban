<template>
	<view class="page">
		<view v-if="falg" class="falg">
			暂无数据~
		</view>
		<view class="row" v-for="(item,index) in info" :key="item.id" @click="goCareer(item.id)">
			<view class="left" :style="colors[index]">
				<text>{{item.careerplanningName}}</text>
			</view>
			<view class="right">
				<text>项目完成 {{rows[index].projectOverSum}}/{{rows[index].projectNum}}</text>
				<text>任务提交 {{rows[index].checkStateNum}}/{{rows[index].planNum}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props:["pageNum"],
		data() {
			return {
				falg: false,
				info: [],
				rows: [],
				colors: ["background-color:#5ec9af","background-color:#f7c868","background-color:#7dafef","background-color:#f07783","background-color:#72ccec","background-color:#fb998e","background-color:#e18cde","background-color:#e28ca8"]
			}
		},
		methods: {
			getInfo(){
				//我的职业
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/my/careerplanning?pageSize=8&pageNum="+this.pageNum,
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					method:"GET",
					dataType:"json",
					success:(res)=> {
						if(res.data.status == 200){
							if(res.data.data.total==0){
								this.falg = true
							}else{
								this.colors = [...this.colors,...this.colors]
								this.info = [...this.info,...res.data.data.rows]
								for(let data of res.data.data.rows){
									this.getCareerplanningProgress(data.id)
								}
							}
						}else{
							uni.showToast({
								title:'请求接口失败'
							})
						}
					}
					
				});
			},
			getCareerplanningProgress(id){
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/my/careerplanningProgress/"+id,
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					method:"GET",
					dataType:"json",
					success:(res)=> {
						if(res.data.status == 200){
							this.rows.push(res.data.data)
						}else{
							uni.showToast({
								title:'请求接口失败'
							})
						}
					}
					
				});
			},
			goCareer(id){
				uni.navigateTo({
					url:"../../pages/career/career?id="+id
				})
			}
		},
		created(){
			this.getInfo();
		}
	}
</script>

<style lang="scss">
	.falg{
		width: 750rpx;
		height: 300rpx;
		line-height: 300rpx;
		text-align: center;
	}
	
	.page{
		background-color: #f2f2f2;
	}
	.row{
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
			letter-spacing: 10rpx;
			display: flex;
			flex-direction: column;
			justify-content: center;
		}
	}
</style>
