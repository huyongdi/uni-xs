<template>
	<view>
		<view class="cu-bar">
			<view class="action">
				<text @click="clearClick">清空全部</text>
			</view>
			<view class="action">
				<text @click="BackClick">完成</text>
			</view>
		</view>
		
		<checkbox-group class="block" @change="CheckboxChange">
			<view class="cu-form-group" v-for="(item,index) in DataList" :key="index">
				<view class="title">{{item.title}}</view>
				<checkbox class="red" :class="item.checked?'checked':''" :checked="item.checked" :value="item.id"></checkbox>
			</view>
		</checkbox-group>
		
		<view class="cu-tabbar-height margin-bottom"></view>
		
		<!-- <view class="padding-left padding-right zai-tabbar-bar" v-if="delArr.length<=0">
			<button class="cu-btn block line-red margin-tb-sm lg" disabled>删除选中</button>
		</view> -->
		
		<view class="padding-left padding-right flex flex-direction zai-tabbar-bar">
			<button :class="delArr.length > 0 ? 'bg-red' : 'block'" class="cu-btn bg-red margin-tb-sm lg" @click="DelClick">删除选中({{delArr.length}})</button>
		</view>
		
	</view>
</template>

<script>
	import _tool from '@/util/tools.js';
	export default {
		data() {
			return {
				DataList: [], 
				delArr: []
			}
		},
		onLoad() {
			this.GetBookList();
		},
		methods: {
			GetBookList() {
				const $that = this
				_tool.GetBookListData().then(function(res) {
					$that.DataList = res ? res : []
				});
			},
			BackClick() {
				uni.navigateBack()
			},
			CheckboxChange (e) {
				let DataList = this.DataList, values = e.detail.value;
				this.delArr = values;
				for (let i = 0; i < DataList.length; ++i) {
					const item = DataList[i];
					if(values.includes(DataList.id)){
						this.$set(item,'checked',true)
					}else{
						this.$set(item,'checked',false)
					}
				}
			},
			DelClick() {
				let that = this;
				if (this.delArr.length > 0) {
					_tool.DelBookList(this.delArr).then(function(res) {
						that.GetBookList();
					});
				}
			},
			clearClick() {
				let that = this;
				_tool.clearStorage().then(function(res) {
					that.GetBookList();
				});
			}
		}
	}
</script>

<style lang="less">
	
</style>
