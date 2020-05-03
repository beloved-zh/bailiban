<template>
	<view>
		<view class="tab">
			<qs-tabs :tabs="tabs" :current="current" animationLineWidth="80" activeColor="#ef5656" @change="change"></qs-tabs>
		</view>
		<view class="main">
			<career v-if="current == 0"></career>
			<project v-if="current == 1" :pageNum="pageNum" ref="project"></project>
			<course v-if="current == 2" :pageNum="pageNum" ref="course"></course>
		</view>
	</view>
</template>

<script>
	import QsTabs from '@/components/QS-tabs/QS-tabs.vue';
	import career from '@/components/sort/career.vue';
	import project from '@/components/sort/project.vue';
	import course from '@/components/sort/course.vue';
	export default {
		data() {
			return {
				tabs:[
					{name:"职业进阶包"},
					{name:"项目MALL"},
					{name:"百里公开课"}
				],
				current: 0,
				pageNum: 1
			}
		},
		methods: {
			change(index){
				this.pageNum = 1;
				this.current = index
			}
		},
		components: {
			QsTabs,
			career,
			project,
			course
		},
		onReachBottom() {
			console.log("页面触底，查询下一页数据")
			this.pageNum++;
			
			if(this.current == 0){
			}else if(this.current == 1){
				this.$refs.project.getInfo()
			}else if(this.current == 2){
				this.$refs.course.getInfo()
			}
		}
	}
</script>

<style lang="scss">
	.tab{
		border: 1rpx solid #eee;
	}
</style>
