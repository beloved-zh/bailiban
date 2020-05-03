<template>
	<view>
		<view class="head">
			<load-line :loadType="loadType" :ballInfo="ballInfo" :loadText="loadText" :loadPercent="loadPercent" :loadTextSize="loadTextSize"></load-line>
		</view>
		<view class="daliy-item" v-for="(item,index) in dailyRows" :key="index" @click="goDailyInfo(item.classId,item.userId,item.weekDate,item.pmName,item.tagName)">
			<view class="daliy-item-head">
				<view class="pmName">项目经理 {{item.pmName}} 布置</view>
				<view class="state" v-if="item.state > 0">已截止</view>
				<view class="state" v-else>{{item.state}}</view>
			</view>
			<view class="xlx"></view>
			<view class="daliy-item-main">
				<view class="left">
					<view class="tagName">{{item.tagName}}</view>
					<view class="weekDate">
						<text class="iconfont icon-shizhong"></text>
						{{item.weekDate.substring(0,10)}} 排课
					</view>
				</view>
				<view class="right">
					<text v-if="item.accuracy != null">{{item.accuracy}}%</text>
					<text v-else>0.00%</text>
					<text>正确</text>
				</view>
			</view>
		</view>
		<view v-if="falg" class="falg">您已到达底部~</view>
	</view>
</template> 

<script>
	import LoadLine from '../../components/load-line/load-line.vue'
	export default {
		data() {
			return {
				/* 
				 * 进度条 https://ext.dcloud.net.cn/plugin?id=1190#detail
				 * 手动去除了外环   loadline.css 87行
				 * 改变颜色		   loadline.css 94行
				 * 修改内边距50rpx       loadline.css 83行
				 * 在原有基础增加球形显示字体： load-line.vue 31行 还有css
				 * 增加百分比显示大小： load-line.vue 28行 82行
				 * 
				 * loadType：进度条类型  ball球形
				 * loadPercent：进度条百分比  不需要 %
				 * ballSize：大小
				 * waveColor：水波颜色
				 * loadText：进度条字体
				 * loadTextSize：百分比显示大小
				 * 
				 */
				loadType: "ball",
				ballInfo:{
					ballSize:130,
					waveColor:'#FFFFFF'
				},
				loadPercent : 68.1,
				loadText : "正确率",
				loadTextSize: "50rpx",
				//每日一测详细数据
				dailyRows:[],
				pageNum:1,
				falg:false
			}
		},
		methods: {
			//获取正确率
			getAccuracy(){
				let userId = uni.getStorageSync("userInfo").id;
				console.log(userId)
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/exam/daily/user/rate?userId="+userId,
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					success: (res) => {
						console.log(res)
						if(res.data.status == 200){
							this.loadPercent = res.data.data.accuracy
						}else{
							uni.showToast({
								title:'请求接口失败'
							})
						}
					}
				})
			},
			//获取每日一册数据
			getDaily(){
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/exam/daily?pageSize=30&pageNum="+this.pageNum,
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					success: (res) => {
						if(res.data.status == 200){
							/* this.dailyRows = res.data.data.rows */
							this.dailyRows = [...this.dailyRows,...res.data.data.rows];
						}else{
							uni.showToast({
								title:'请求接口失败'
							})
						}
					}
				})
			},
			goDailyInfo(classId,userId,weekDate,pmName,tagName){
				weekDate = weekDate.replace(new RegExp("-","gm"),"/");// 转换格式:yyyy/mm/dd hh:mm:ss
				weekDate = (new Date(weekDate)).getTime(); //得到毫秒数
				uni.navigateTo({
					url: "./dailyInfo?classId="+classId+"&userId="+userId+"&weekDate="+weekDate+"&pmName="+pmName+"&tagName="+tagName
				})
				 /* /api/admin/miniprogram/exam/daily/survey/get?classId=793&userId=1957&weekDate=1585065600000 */
			}
		},
		onLoad() {
			this.getAccuracy()
			this.getDaily()
		},
		components:{
			LoadLine,
		},
		onReachBottom() {
			if(30*this.pageNum > this.dailyRows.length){
				this.falg = true
				return false;
			}
			console.log("页面触底，查询下一页数据")
			this.pageNum++;
			this.getDaily()
		}
	}
</script>

<style lang="scss">
	page{
		background-color: #f2f2f2;
	}
	.head{
		background-color: #FFFFFF;
	}
	.daliy-item{
		width: 700rpx;
		height: 300rpx;
		margin: 20rpx 25rpx 10rpx 25rpx;
		background-color: #FFFFFF;
		border-radius: 10rpx;
		.daliy-item-head{
			width: 650rpx;
			height: 80rpx;
			line-height: 80rpx;
			/* border-bottom: 1rpx solid #eee; */
			margin: 0 auto;
			display: flex;
			flex-direction: row;
			justify-content: space-between;
			color: #666666;
		}
		.xlx{
			width: 700rpx;
			height: 1rpx;
			border-bottom: 1rpx solid #eee;
		}
		.daliy-item-main{
			width: 650rpx;
			height: 220rpx;
			margin: 0 auto;
			display: flex;
			flex-direction: row;
			justify-content: space-between;
			.left{
				height: 80%;
				margin: auto 0;
				display: flex;
				flex-direction: column;
				justify-content: space-between;
				.tagName{
					font-size: 35rpx;
					font-weight: bold;
				}
				.weekDate{
					font-size: 28rpx;
					color: #666666;
				}
			}
			.right{
				height: 80%;
				margin: auto 0;
				//防止内容过长自动换行
				display: inline-block;
			    white-space: nowrap;
				line-height: 150rpx;
				text:nth-child(1){
					font-size: 50rpx;
				}
				text:nth-child(2){
					font-size: 28rpx; 
				}
			}
		}
	}

	.falg{
		width: 750rpx;
		height: 100rpx;
		line-height: 100rpx;
		text-align: center;
		color: #a1a1a1;
	}
</style>
