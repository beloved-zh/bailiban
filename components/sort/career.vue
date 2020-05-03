<template>
	<view class="page">
		<view class="row">
			<view class="item" v-for="(item,index) in career" :key="item.id" @click="goCareer(item.id)">
				<view class="careerplanningName" :style="colors[index]">
					<text>{{item.careerplanningName}}</text>
				</view>
				<view class="careerplanningPrice">
					<text v-if="item.careerplanningPrice == null">免费</text>
					<text>{{item.careerplanningPrice == 0?'免费':item.careerplanningPrice}}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				career:[],
				colors: ["background-color:#5ec9af","background-color:#f7c868","background-color:#7dafef","background-color:#f07783","background-color:#72ccec","background-color:#fb998e","background-color:#e18cde","background-color:#e28ca8"]
			}
		},
		methods: {
			getInfo(){
				uni.request({
					url:"https://api.bailiban.com/api/admin/open/v1/miniprogram/careerplanning/careerplanningPage",
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					method:"GET",
					dataType:"json",
					success:(res)=> {
						if(res.data.status == 200){
							this.career = res.data.data.rows
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
			goCareer(id){
				uni.navigateTo({
					url:"../../pages/career/career?id="+id
				})
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
				box-shadow: 0rpx 5rpx 5rpx #f4f4f4;
				.careerplanningName{
					width: 330rpx;
					height: 200rpx;
					color: #FFFFFF;
					text-align: center;
					display: flex;
					flex-direction: column;
					justify-content: center;
				}
				.careerplanningPrice{
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

