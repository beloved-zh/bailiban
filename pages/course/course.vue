<template>
	<view>
		<view class="tab">
			<qs-tabs :tabs="tabs" :current="current" animationLineWidth="80" activeColor="#ef5656" @change="change"></qs-tabs>
		</view>
		<view class="main">
			<course-info v-if="(current == 0)&&(id != '')" :cid="id"></course-info>
			<chapters-list v-if="(current == 1)&&(id != '')" :cid="id"></chapters-list>
			<recommend v-if="(current == 2)&&(id != '')" :cid="id"></recommend>
		</view>
	</view>
</template>

<script>
	import QsTabs from '@/components/QS-tabs/QS-tabs.vue';
	import CourseInfo from '@/components/course/courseInfo.vue';
	import chaptersList from '@/components/course/chaptersList.vue';
	import recommend from '@/components/course/recommend.vue';
	export default {
		data() {
			return {
				id:"",
				tabs:[
					{name:"课程简介"},
					{name:"目录"},
					{name:"推荐课程"}
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
			CourseInfo,
			chaptersList,
			recommend
		}
	}
</script>

<style lang="scss">
	.tab{
		border: 1rpx solid #eee;
	}
</style>
