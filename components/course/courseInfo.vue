<template>
	<view class="courseInfo">
		<rich-text :nodes="info.courseContent" space="nbsp"></rich-text>
	</view>
</template>

<script>
	export default {
		props:["cid"],
		data() {
			return {
				info:[]
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
							this.info = res.data.data.course
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
			this.getInfo()
		}
	}
</script>

<style lang="scss">
	.courseInfo{
		margin: 25rpx;
	}	
</style>
