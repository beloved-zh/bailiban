<template>
	<view class="page">
		<view class="row">
			<view class="item" v-for="(item,index) in courseList" :key="item.id" @click="goCoures(item.id)">
				<view class="courseName" :style="colors[index]">
					<text>{{item.courseName}}</text>
				</view>
				<view class="coursePrice">
					<text>{{item.coursePrice == 0?'免费':item.coursePrice}}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props:["cid"],
		data() {
			return {
				courseList: [],
				colors: ["background-color:#5ec9af","background-color:#f7c868","background-color:#7dafef","background-color:#f07783","background-color:#72ccec","background-color:#fb998e","background-color:#e18cde","background-color:#e28ca8"]
			}
		},
		methods: {
			getInfo(){
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/course/info?id="+this.cid,
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					method:"GET",
					dataType:"json",
					success:(res)=> {
						if(res.data.status == 200){
							this.courseList = res.data.data.courseList
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
			goCoures(id){
				uni.navigateTo({
					url:"../../pages/course/course?id="+id
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
	.page{
		background-color: #FFFFFF;
		.row{
			width: 700rpx;
			margin: 25rpx;
			display: flex;
			flex-direction: row;
			justify-content: space-between;
			flex-wrap: wrap;
			.item{
				width: 330rpx;
				height: 250rpx;
				border: 1rpx solid #f8f8f8;
				margin-top: 25rpx;
				border-radius: 10rpx;
				.courseName{
					width: 330rpx;
					height: 200rpx;
					color: #FFFFFF;
					text-align: center;
					display: flex;
					flex-direction: column;
					justify-content: center;
				}
				.coursePrice{
					width: 330rpx;
					height: 50rpx;
					line-height: 50rpx;
					background-color: #FFFFFF;
					font-size: 20rpx;
					color: $color;
					text-align: right;
					text{
						padding-right: 30rpx;
					}
				}
			}
		}
	}
</style>
