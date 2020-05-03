<template>
	<view>
		<view class="tab">
			<qs-tabs :tabs="tabs" :current="current" animationLineWidth="50" minWidth="375" activeColor="#ef5656" @change="change"></qs-tabs>
		</view>
		<view class="main">
			<career-info v-if="(current == 0)&&(id != '')" :cid="id"></career-info>
			<career-stage v-if="(current == 1)&&(id != '')" :cid="id"></career-stage>
		</view>
	</view>
</template>

<script>
	import QsTabs from '@/components/QS-tabs/QS-tabs.vue';
	import careerInfo from '@/components/sort/careerInfo.vue';
	import careerStage from '@/components/sort/careerStage.vue';
	export default {
		data() {
			return {
				id: "",
				tabs:[
					{name:"进阶包介绍"},
					{name:"进阶路线"}
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
			careerInfo,
			careerStage
		}
	}
</script>

<style lang="scss">
	.tab{
		border: 1rpx solid #eee;
	}
</style>
