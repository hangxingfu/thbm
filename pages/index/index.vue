<template>
	<view class="content">
		<!-- 参数 
		* @property {ObjectArray} list = [] 列表数据,数据格式[{"name": "花呗","icon": "/static/img/1.png",}]
		* @property {Boolean} feedbackGenerator = [true|false] 是否开启拖动触感反馈  
		* @property {Boolean} longTouch = [true|false] 是否开启长按拖动  
		* @property {Boolean} autoScroll = [true|false] 是否拖拽至边缘自动滚动列表  
		* @property {Number} longTouchTime = [] 选填,触发长按时长,单位:ms,默认350ms
		* @property {Number} listHeight = 0 选填,可拖动列表整体的高度,单位:px,默认等于窗口高度 
		* @property {Number} rowHeight = 44 必填,行高,单位:px,默认44px
		* @event {Function} change 行位置发生改变时触发事件 返回值:{index:'原始下标',moveTo:'被拖动到的下标',moveRow:'拖动行数据'}   
		* @event {Function} confirm 拖拽结束且行位置发生了改变触发事件 返回值:{index:'原始下标',moveTo:'被拖动到的下标',moveRow:'拖动行数据',list:'整个列表拖动后的数据'}  
		-->
		<HM-dragSorts
			ref="dragSorts"
			:list="list"
			:longTouch="true"
			:feedbackGenerator="true"
			:rowHeight="44"
			@onclick="onclick"
			@change="change"
			@confirm="confirm"
		></HM-dragSorts>

		<view class="test_btn">
			<view @click="push">push</view>
			<view @click="push">push</view>
			<view @click="unshift">unshift</view>
			<view @click="splice">splice</view>
		</view>
	</view>
</template>

<script>
import { sum, hello } from 'exercise';

export default {
	data() {
		return {
			/*
					0.数据结构是个数组，数据格式[{"name": "花呗","icon": "/static/img/1.png",}]。
					
					1.动态修改list数据，组件会响应变化，但拖动排序后不会改变list数据，无双向绑定
					  所以，要特别注意，在动态修改list数据时候，请使用组件的push，unshift，splice方法
					  建议是，list一次性赋值，后续的修改都通过$refs调用push，unshift，splice方法修改
					  
					n.最后，组件你下载了，代码你也看了，不给个五星好评，我可要打人了啊  (╬￣皿￣)=○  ヽ(#`Д´)ﾉ
				*/
			tmplist: [
				{
					name: '花呗',
					icon: '/static/img/1.png'
				},
				{
					name: '余额宝',
					icon: '/static/img/2.png',
					disabled: true
				},
				{
					name: '账户余额',
					icon: '/static/img/3.png'
				},
				{
					name: '交通银行信用卡(0001)',
					icon: '/static/img/4.png'
				},
				{
					name: '中国建设银行信用卡(4401)',
					icon: '/static/img/5.png'
				},
				{
					name: '网商储蓄卡(7223)',
					icon: '/static/img/6.png'
				},
				{
					name: '账户余额',
					icon: '/static/img/3.png'
				},
				{
					name: '交通银行信用卡(0001)',
					icon: '/static/img/4.png'
				},
				{
					name: '中国建设银行信用卡(4401)',
					icon: '/static/img/5.png'
				},
				{
					name: '网商储蓄卡(7223)',
					icon: '/static/img/6.png'
				}
			],
			list: []
		};
	},
	onLoad() {
		console.log(hello('zhangsan'), sum(1, 2));

		let listdata = [];
		// 循环数据
		listdata.length = 100;
		for (let i = 0; i < listdata.length; i++) {
			let index = i % 10;
			listdata[i] = JSON.parse(JSON.stringify(this.tmplist[index]));
			listdata[i].id = i;
		}
		this.list = listdata;
	},
	methods: {
		push() {
			// 和数组的push使用方法一致，可以push单行，也可以push多行  push()会返回修改后的list
			let tmplist = this.$refs.dragSorts.push({
				name: 'push行',
				icon: '/static/img/2.png'
			});
			console.log('push修改后的list: ', tmplist);
			// 多行
			// let tmp_list = [
			// 	{
			// 		"name": "push行1",
			// 		"icon": "/static/img/2.png"
			// 	},
			// 	{
			// 		"name": "push行2",
			// 		"icon": "/static/img/3.png"
			// 	}
			// ]
			// this.$refs.dragSorts.push(...tmp_list);
		},
		unshift() {
			// 和数组的unshift使用方法一致，可以unshift单行，也可以unshift多行 unshift()会返回修改后的list
			let tmplist = this.$refs.dragSorts.unshift({
				name: 'unshift行',
				icon: '/static/img/2.png'
			});
			console.log('unshift修改后的list: ', tmplist);

			// 多行
			// let tmp_list = [
			// 	{
			// 		"name": "unshift行1",
			// 		"icon": "/static/img/2.png"
			// 	},
			// 	{
			// 		"name": "unshift行2",
			// 		"icon": "/static/img/3.png"
			// 	}
			// ]
			// this.$refs.dragSorts.unshift(...tmp_list);
		},
		splice() {
			// 和数组的unshift使用方法一致 splice()会返回修改后的list 下标1开始删除1个并在下标1位置插入行
			let tmplist = this.$refs.dragSorts.splice(1, 1, {
				name: 'splice行',
				icon: '/static/img/2.png'
			});
			console.log('splice修改后的list: ', tmplist);

			// 插入多行
			// let tmp_list = [
			// 	{
			// 		"name": "unshift行1",
			// 		"icon": "/static/img/2.png"
			// 	},
			// 	{
			// 		"name": "unshift行2",
			// 		"icon": "/static/img/3.png"
			// 	}
			// ]
			// this.$refs.dragSorts.splice(1,1,...tmp_list);
		},
		onclick(e) {
			console.log('=== onclick start ===');
			console.log('被点击行: ', JSON.stringify(e));
			console.log('=== onclick end ===');
		},
		change(e) {
			console.log('=== change start ===');
			console.log('被拖拽行: ', JSON.stringify(e.moveRow));
			console.log('原始下标: ', e.index);
			console.log('被拖拽到: ', e.moveTo);
			console.log('=== change end ===');
		},
		confirm(e) {
			console.log('=== confirm start ===');
			console.log('被拖拽行: ', JSON.stringify(e.moveRow));
			console.log('原始下标: ', e.index);
			console.log('被拖拽到: ', e.moveTo);
			console.log('排序后的list: ', e.list);
			// 如果需要删除列表行，必须要排序后，同步一下排序后的list到页面list
			// 因为排序后页面的list和组件内的list已经不一样，不同步list，直接删除页面的list中项目，达不到实际效果。
			console.log('=== confirm end ===');
		}
	}
};
</script>
<style lang="scss" scoped>
//scoped css只在当前页生效 不影响子组件
.content {
}

.test_btn {
	position: fixed;
	bottom: 0;
	width: 100%;
	height: 44px;
	display: flex;
	align-items: center;
	background-color: bisque;

	> view {
		width: 100%;
		height: 44px;
		display: flex;
		align-items: center;
		justify-content: center;
		border-left: solid 1px #eee;

		&:first-child {
			border-left: none;
		}
	}
}
</style>