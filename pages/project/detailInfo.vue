<template>
	<view>
		<rich-text :nodes="info.context" space="nbsp"></rich-text>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id: "",
				info: []
			}
		},
		methods: {
			getInfo(){
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/project/plan/detail/"+this.id,
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					method:"GET",
					dataType:"json",
					success:(res)=> {
						console.log(res)
						if(res.data.status == 200){
							this.info = res.data.data
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
		onLoad(option) {
			this.id = option.id;
			this.getInfo()
		},
	}
</script>

<style>
	rich-text{
		margin: 25rpx;
	}
</style>
