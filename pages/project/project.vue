<template>
	<view>
		<view class="tab">
			<qs-tabs :tabs="tabs" :current="current" animationLineWidth="80" activeColor="#ef5656" @change="change"></qs-tabs>
		</view>
		<view class="main">
			<projrct-info v-if="(current == 0)&&(id != '')" :pid="id"></projrct-info>
			<project-detail v-if="(current == 1)&&(id != '')" :pid="id"></project-detail>
			<project-video v-if="(current == 2)&&(id != '')" :pid="id"></project-video>
		</view>
	</view>
</template>

<script>
	import QsTabs from '@/components/QS-tabs/QS-tabs.vue';
	import projrctInfo from '@/components/project/projrctInfo.vue';
	import projectDetail from '@/components/project/projectDetail.vue';
	import projectVideo from '@/components/project/projectVideo.vue';
	export default {
		data() {
			return {
				id:"",
				tabs:[
					{name:"项目介绍"},
					{name:"任务列表"},
					{name:"项目精讲"}
				],
				current: 0
			}
		},
		methods: {
			change(index){
				this.current = index
			}
		},
		onLoad(option) {
			this.id = option.id;
		},
		components: {
			QsTabs,
			projrctInfo,
			projectDetail,
			projectVideo
		}
	}
</script>

<style lang="scss">
	.tab{
		border: 1rpx solid #eee;
	}
</style>
