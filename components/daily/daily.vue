<template>
	<view>
		<view class="rows" v-for="(item,index) in dailys" :key="index">
			<view class="tagName" v-if="item.tagName.length > 13">{{item.tagName.substring(0,13)}}...</view>
			<view class="tagName" v-else>{{item.tagName}}</view>
			<view class="weekDate">{{item.weekDate.substring(0,10)}}</view>
			<view class="state" style="color: #00B26A;" v-if="item.state == 0">开始测试</view>
			<view class="state" @click="goDailyInfo(item.classId,item.userId,item.weekDate,item.pmName,item.tagName)" style="color: #00B26A;" v-else-if="item.state == 1">查看结果</view>
			<view class="state" v-else-if="item.state == 2">{{item.state}}</view>
			<view class="state" v-else-if="item.state == 3">未开放</view>
			<view class="state" v-else-if="item.state == 4">{{item.state}}</view>
			<view class="state" v-else-if="item.state == 5">已过期</view>
		</view>
		<view class="falg" v-if="falg">亲，已经到底了哦~</view>
	</view>
</template>

<script>
	export default {
		props:["pageNum"],
		data() {
			return {
				"dailys":[],
				falg: false
			}
		},
		methods: {
			//查看结果
			goDailyInfo(classId,userId,weekDate,pmName,tagName){
				weekDate = weekDate.replace(new RegExp("-","gm"),"/");// 转换格式:yyyy/mm/dd hh:mm:ss
				weekDate = (new Date(weekDate)).getTime(); //得到毫秒数
				uni.navigateTo({
					url: "../../pages/daily/dailyInfo?classId="+classId+"&userId="+userId+"&weekDate="+weekDate+"&pmName="+pmName+"&tagName="+tagName
				})
			},
			getDailys(pageNum){
				//每日一册
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/exam/daily?pageSize=15&pageNum="+this.pageNum,
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					method:"GET",
					dataType:"json",
					success:(res)=> {
						if(res.data.status == 200){
							this.dailys = [...this.dailys,...res.data.data.rows];
							if(res.data.data.rows.length < 15){
								this.falg = true
							}
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
			this.getDailys()
		}
		
	}
</script>

<style lang="scss">
	.rows{
		width: 700rpx;
		margin: 20rpx 25px 0 25rpx;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		.tagName{
			width: 50%;
		}
		.weekDate{
			width: 30%;
		}
		.state{
			width: 20%;
			color: #808080;
		}
	}
	.falg{
		width: 750rpx;
		height: 80rpx;
		background-color: #f2f2f2;
		color: $color;
		font-size: 20rpx;
		text-align: center;
		line-height: 80rpx;
		margin-top: 20rpx;
	}
</style>
