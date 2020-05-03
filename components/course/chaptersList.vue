<template>
	<view class="list">
		<view class="row" v-for="row in list" :key="row.id">
			<view class="chapterName">
				<text class="iconfont icon-fenlei"></text>
				<text>{{row.chapterName}}</text>
			</view>
			<view class="children" v-for="item in row.children" :key="item.id">
				<text class="iconfont icon-yousanjiaoxing"></text>
				<text>{{item.chapterName}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props:["cid"],
		data() {
			return {
				list:[]
			}
		},
		methods: {
			getList(){
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/course/chaptersList?id="+this.cid,
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					method:"GET",
					dataType:"json",
					success:(res)=> {
						if(res.data.status == 200){
							this.list = res.data.data.chaptersList
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
			}
		},
		//组件调用后
		created(){
			this.getList()
		}
	}
</script>

<style lang="scss">

	.list{
		width: 700rpx;
		margin: 25rpx;
		.row{
			margin-top: 20rpx;
			.chapterName{
				font-size: 35rpx;
				font-weight: bold;
				line-height: 35rpx;
				text:nth-child(2){
					margin-left: 20rpx;
				}
			}
			.children{
				width: 100%;
				height: 70rpx;
				margin-top: 25rpx;
				padding-left: 25rpx;
				line-height: 70rpx;
				background-color: #eeeeee;
				.icon-yousanjiaoxing{
					color: #bfbfbf;
				}
				text:nth-child(2){
					margin-left: 20rpx;
				}
			}
		}
	}
</style>
