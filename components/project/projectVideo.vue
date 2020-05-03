<template>
	<view>
		<view v-if="falg">暂无</view>
	</view>
</template>

<script>
	export default {
		props:["pid"],
		data() {
			return {
				falg:false
			}
		},
		methods: {
			getVideo(){
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/project/video/list?projectId="+this.pid,
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					method:"GET",
					dataType:"json",
					success:(res)=> {
						console.log(res)
						if(res.data.status == 200){
							if(res.data.data.length == 0){
								this.falg = true
							}else{
								
							}
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
			this.getVideo()
		}
	}
</script>

<style>

</style>
