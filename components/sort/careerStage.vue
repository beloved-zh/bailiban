<template>
	<view class="page">
		<view class="remarks">
			{{info.careerPlanning.remarks}}
		</view>
		<view class="stage" v-for="stage in info.stageList" :key="stage.id">
			<view class="stagename">
				{{stage.stagename}}
			</view>
			<view class="row">
				<!-- 0是课程 1是项目 -->
				<view class="item"  v-for="item in stage.relationInfoList" :key="item.id" >
					<view :style="colors[getRandom()]" v-if="item.type == 0" @click="goCourse(item.typeId)">
						<text>{{item.typeName}}</text>
					</view>
					<view :style="colors[getRandom()]" v-else-if="item.type == 1" @click="goProject(item.typeId)">
						<text>{{item.typeName}}</text>
					</view>
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
				info: [],
				colors: ["background-color:#5ec9af","background-color:#f7c868","background-color:#7dafef","background-color:#f07783","background-color:#72ccec","background-color:#fb998e","background-color:#e18cde","background-color:#e28ca8"]
			}
		},
		methods: {
			getInfo(){
				uni.request({
					url:"https://api.bailiban.com/api/admin/miniprogram/careerplanning/info/"+this.cid,
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
			},
			getRandom(){
				return Math.floor(Math.random()*8);
			},
			goCourse(id){
				uni.navigateTo({
					url:"../../pages/course/course?id="+id
				})
			},
			goProject(id){
				uni.navigateTo({
					url:"../../pages/project/project?id="+id
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
		width: 700rpx;
		margin: 25rpx;
		.remarks{
			color: #8296b8;
			line-height: 45rpx;
		}
		.stage{
			width: 700rpx;
			margin-top: 30rpx;
			text-align: center;
			.stagename{
				width: 700rpx;
				font-size: 35rpx;
				font-weight: bold;
			}
			.row{
				width: 700rpx;
				/* margin-top: 25rpx; */
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
					background-color: $color;
					color: #FFFFFF;
					view{
						width: 330rpx;
						height: 250rpx;
						display: flex;
						flex-direction: column;
						justify-content: center;
					}
				}
			}
		}
	}

</style>
