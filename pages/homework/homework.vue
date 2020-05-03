<template>
	<view>
		<uni-segmented-control :current="current" :values="items" @clickItem="onClickItem" style-type="text" active-color="#12c494"></uni-segmented-control>
		<view class="work" v-if="falg">
			<view class="work-time" v-for="(item,index) in homeworks" :key="item.id">
				<view class="work-main">
					<view class="left">
						<view class="work-time-name">{{item.firstTagName}}-{{item.secondTagName}}</view>
						<view class="work-time-sum">
							<text>任务:{{item.workSum}}</text>
							<text>提交:{{item.submitSum}}</text>
						</view>
					</view>
					<view class="right">
						<button @click="goHomeworkItem(item.classId,item.createBy,item.firstTagId,item.secondTagId)">去查看</button>
					</view>
				</view>
			</view>
		</view>
		<view v-else class="not">
			<image src="../../static/icon/noHomework.png"></image>
			<view>暂无作业~</view>
		</view>
	</view>
</template>

<script>
	import uniSegmentedControl from "@/components/uni-segmented-control/uni-segmented-control.vue"
	export default {
	    components: {uniSegmentedControl},
	    data() {
	        return {
	            items: ['上机作业','题库作业','项目作业'],
	            current: 0,
				homeworks: [],
				page: 1,
				falg: false
	        }
	    },
	    methods: {
	        onClickItem(e) {
	            if (this.current !== e.currentIndex) {
	                this.current = e.currentIndex;
					this.homeworks = []
					this.page = 1
					this.getHomework()
	            }
	        },
			getHomework(){
				uni.request({
					url:"https://api.bailiban.com/api/admin/apis/user/homework?type="+(this.current+1)+"&page="+this.page+"&limit=10",
					header:{
						"Authorization":uni.getStorageSync("blbUI-token")
					},
					success: (res) => {
						console.log(res.data)
						if(res.data.status == 200){
							if(res.data.data.total == 0){
								this.falg = false
							}else{
								this.falg = true
								this.homeworks = [...this.homeworks,...res.data.data.rows];
							}
						}else{
							uni.showToast({
								title:'请求接口失败'
							})
						}
					}
				})
			},
			goHomeworkItem(classId,createBy,firstTagId,secondTagId){
				uni.navigateTo({
					url: "homeworkItem?classId="+classId+"&createBy="+createBy+"&firstTagId="+firstTagId+"&secondTagId="+secondTagId
				})
			}
	    },
		onLoad() {
			this.getHomework()
		},
		onReachBottom() {
			console.log("页面触底，查询下一页数据")
			this.page++;
			this.getHomework()
		}
		
	}
	
</script>

<style lang="scss">
	page{
		background-color: #f2f2f2;
	}
	.work{
		width: 700rpx;
		margin:25rpx;
		.work-time{
			background-color: #FFFFFF;
			width: 100%;
			height: 200rpx;
			border-radius: 7px;
			margin-top: 25rpx;
			.work-main{
				width: 640rpx;
				height: 160rpx;
				/* margin: auto; */
				padding: 20rpx 30rpx;
				display: flex;
				flex-direction: row;
				justify-content: space-between;
				align-items: space-between;
				.left{
					display: flex;
					flex-direction: column;
					justify-content: space-between;
					.work-time-name{
						font-size: 32rpx;
						font-weight: bold;
					}
					.work-time-sum{
						font-size: 28rpx; 
						text:nth-child(1){
							color: #666666;
						}
						text:nth-child(2){
							margin-left: 50rpx;
							color: $color;
						}
					}
				}
				.right{
					margin: auto 0;
					button{
						color: #FFFFFF;
						background-color: $color;
						border-radius: 30px;
						width: 200rpx;
						height: 80rpx;
						font-size: 28rpx;
						line-height: 80rpx;
					}
				} 
			}
		}
	}
	.not{
		width: 100%;
		height: 750rpx;
		text-align: center;
		image{
			width: 450rpx;
			height: 300rpx;
			margin: 300rpx 150rpx 50rpx 150rpx;
		}
		view{
			font-size: 20rpx;
		}
	}
</style>
