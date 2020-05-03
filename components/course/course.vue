<template>
	<view class="page">
		<view class="row" v-for="(item,index) in course" :key="item.id" @click="goCoures(item.id)">
			<view class="left" :style="colors[index]">{{item.courseName}}</view>
			<view class="right">{{item.courseName}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		props:["pageNum"],
		data() {
			return {
				course: [],
				colors: ["background-color:#5ec9af","background-color:#f7c868","background-color:#7dafef","background-color:#f07783","background-color:#72ccec","background-color:#fb998e","background-color:#e18cde","background-color:#e28ca8"]
			}
		},
		methods: {
			getCoures(){
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/my/course?pageSize=8&pageNum="+this.pageNum,
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					method:"GET",
					dataType:"json",
					success:(res)=> {
						if(res.data.status == 200){
							this.colors = [...this.colors,...this.colors]
							this.course = [...this.course,...res.data.data.rows];
						}else{
							uni.showToast({
								title:'请求接口失败'
							})
						}
					}
				});
			},
			goCoures(id){
				uni.navigateTo({
					url:"../../pages/course/course?id="+id
				});
			}
		},
		//组件调用后
		created(){
			this.getCoures()
		}
	}
</script>

<style lang="scss">
	.page{
		background-color: #f2f2f2;
	}
	.row{
		width: 750rpx;
		height: 150rpx;
		margin-top: 10rpx;
		display: flex;
		flex-direction: row;
		background-color: #FFFFFF;
		.left{
			width: 40%;
			height: 150rpx;
			border-radius: 10rpx;
			text-align: center;
			color: #FFFFFF;
			line-height: 150rpx;
		}
		.right{
			padding-left: 25rpx;
			line-height: 150rpx;
		}
	}
</style>
