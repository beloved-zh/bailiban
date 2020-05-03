<template>
	<view class="detail">
		<view class="row" v-for="item in detail" :key="item.id" @click="goDetailInfo(item.id)">
			<text class="iconfont icon-yousanjiaoxing"></text>
			<text class="name">{{item.name}}</text>
		</view>
	</view>
</template>

<script>
	export default {
		props:["pid"],
		data() {
			return {
				detail:[]
			}
		},
		methods: {
			getDetail(){
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/project/plan/detail/list?projectId="+this.pid,
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					method:"GET",
					dataType:"json",
					success:(res)=> {
						console.log(res)
						if(res.data.status == 200){
							this.detail = res.data.data
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
			goDetailInfo(id){
				uni.navigateTo({
					url:"../../pages/project/detailInfo?id="+id
				})
			}
		},
		//组件调用后
		created(){
			this.getDetail()
		}
	}
</script>

<style lang="scss">
	
	.detail{
		width: 700rpx;
		margin: 25rpx;
		.row{
			width: 100%;
			height: 80rpx;
			line-height: 80rpx;
			margin-bottom: 25rpx;
			background-color: #eeeeee;
			.icon-yousanjiaoxing{
				color: #FFFFFF;
				margin: 0 25rpx 0 25rpx;
			}
			.name{
				color: #000000;
			}
		}
		.active{
			width: 100%;
			height: 80rpx;
			margin-bottom: 25rpx;
			background-color: #12c494;
			.icon-yousanjiaoxing{
				font-size: 60rpx;
				color: #FFFFFF;
				line-height: 60rpx;
			}
			.name{
				color: #FFFFFF;
			}
		}
	}
</style>
