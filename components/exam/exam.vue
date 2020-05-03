<template>
	<view>
		<view class="row" v-for="item in exam" :key="item.id">
			<view class="paperName" v-if="item.paperName.length < 15">{{item.paperName}}</view>
			<view class="paperName" v-else>{{item.paperName.substring(0,15)}}...</view>
			<view class="createDate">{{item.createDate.substring(0,10)}}</view>
			<view class="result">{{item.result}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				exam: []
			}
		},
		methods: {
			getExam(){
				//我的考试
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/my/exam",
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					method:"GET",
					dataType:"json",
					success:(res)=> {
						if(res.data.status == 200){
							this.exam = res.data.data
						}else{
							uni.showToast({
								title:'请求接口失败'
							})
						}
					}
				});
			}
		},
		//组件调用后
		created(){
			this.getExam()
		}
	}
</script>

<style lang="scss">
	.row{
		width: 700rpx;
		margin: 25px 25px 25px 0;
		display: flex;
		flex-direction: row;
		.paperName{
			width: 60%;
		}
		.createDate{
			width: 30%;
		}
	}
	
</style>
