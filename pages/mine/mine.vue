<template>
	<view class="container">
		<!-- 顶部搜索栏 -->
		<view class="search-header">
			<view class="search-bar">
				<icon type="search" size="14" color="#999"></icon>
				<input type="text" v-model="keyword" placeholder="请输入项目名称" placeholder-class="placeholder" />
				<icon v-if="keyword" type="clear" size="14" color="#ccc" @click="keyword=''"></icon>
			</view>
			<text class="cancel-btn">取消</text>
		</view>

		<!-- 横向滚动标签栏 -->
		<scroll-view scroll-x="true" class="tabs-container" :show-scrollbar="false">
			<view class="tab-item" 
				v-for="(tab, index) in tabs" 
				:key="index" 
				:class="{ active: currentTab === index }"
				@click="currentTab = index">
				<text>{{ tab }}</text>
				<view class="tab-line" v-if="currentTab === index"></view>
			</view>
		</scroll-view>

		<!-- 列表内容 -->
		<view class="list-container">
			<view class="card" v-for="(item, index) in currentList" :key="index">
				<view class="card-header">
					<!-- 动态绑定标题颜色 -->
					<text class="card-type" :style="{ color: item.titleColor }">{{ item.type }}</text>
					<view class="detail-link" @click="goToDetail(item)">
						<text>详情</text>
						<text class="arrow">></text>
					</view>
				</view>
				<view class="divider"></view>
				<view class="card-body">
					<view class="info-row">
						<text class="label">项目名称：</text>
						<text class="value">{{ item.projectName }}</text>
					</view>
					<view class="info-row">
						<text class="label">缴纳机构：</text>
						<text class="value">{{ item.institution }}</text>
					</view>
					<view class="info-row">
						<text class="label">申请状态：</text>
						<text class="value">{{ item.status }}</text>
					</view>
				</view>
			</view>
		</view>

		<!-- 悬浮按钮 -->
		<view class="fab-button">
			<text class="plus-icon">+</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				keyword: '',
				currentTab: 0,
				tabs: ['保单/保函', '现金存单', '银行纸质保函', '300万以下项目'],
				// Tab 0: 保单/保函
				list0: [
					{
						type: '生效中',
						titleColor: '#76EE00', // 鲜艳的草绿色
						projectName: '科创中心实验成果转化基地施工项目',
						institution: '大家财产保险有限责任公司',
						amount: '200,000.00'
					},
					{
						type: '已启用',
						titleColor: '#FF3030', // 鲜艳的红色
						projectName: '科创中心实验成果转化基地施工项目',
						institution: '中国人寿财产保险股份有限公司',
						amount: '660,000.00'
					},
					{
						type: '已返还',
						titleColor: '#00BFFF', // 深天蓝
						projectName: '科创中心实验成果转化基地施工项目',
						institution: '永诚财产保险股份有限公司',
						amount: '100,000.00'
					}
				],
				// Tab 1/2/3: 现金存单、银行纸质保函、300万以下项目（复用相同数据结构）
				list1: [
					{
						type: '保险保单',
						titleColor: 'black',
						projectName: '科创中心实验成果转化基地施工项目',
						institution: '大家财产保险有限责任公司',
						status: '申请通过'
					},
					{
						type: '现金存单',
						titleColor: 'black',
						projectName: '科创中心实验成果转化基地施工项目',
						institution: '建设银行辽宁丹东支行',
						status: '审核中'
					},
					{
						type: '银行保函',
						titleColor: 'black',
						projectName: '科创中心实验成果转化基地施工项目',
						institution: '建设银行辽宁丹东支行',
						status: '申请通过'
					}
				]
			}
		},
		computed: {
			currentList() {
				// Tab 1/2/3 都复用 list1 的数据
				if (this.currentTab === 0) {
					return this.list0;
				}
				return this.list1;
			}
		},
		onShow() {
			uni.setNavigationBarTitle({
				title: '我的保证金'
			});
		},
		methods: {
			goToDetail(item) {
				uni.navigateTo({
					url: '/pages/mine/detail?id=' + (item.id || 0)
				});
			}
		}
	}
</script>

<style>
	page {
		background-color: #F8F8F8;
	}
	
	.container {
		padding-bottom: 100rpx;
	}

	/* 搜索栏 */
	.search-header {
		background-color: #FFFFFF;
		padding: 20rpx 30rpx;
		display: flex;
		align-items: center;
		position: sticky;
		top: 0;
		z-index: 100;
	}

	.search-bar {
		flex: 1;
		background-color: #F5F5F5;
		height: 72rpx;
		border-radius: 36rpx;
		display: flex;
		align-items: center;
		padding: 0 20rpx;
	}

	.search-bar input {
		flex: 1;
		font-size: 28rpx;
		margin-left: 10rpx;
		color: #333;
	}
	
	.placeholder {
		color: #999;
	}

	.cancel-btn {
		font-size: 30rpx;
		color: #666;
		margin-left: 20rpx;
	}

	/* 标签栏 */
	.tabs-container {
		background-color: #FFFFFF;
		white-space: nowrap;
		padding: 0 10rpx;
		border-bottom: 1rpx solid #EEEEEE;
	}

	.tab-item {
		display: inline-block;
		padding: 20rpx 30rpx;
		position: relative;
		font-size: 30rpx;
		color: #666;
	}

	.tab-item.active {
		color: #333;
		font-weight: bold;
	}

	.tab-line {
		position: absolute;
		bottom: 10rpx;
		left: 50%;
		transform: translateX(-50%);
		width: 48rpx; /* 稍微加宽以匹配视觉 */
		height: 6rpx;
		background-color: #C8A068; /* 金色线条 */
		border-radius: 4rpx;
	}

	/* 列表卡片 */
	.list-container {
		padding: 20rpx;
	}

	.card {
		background-color: #FFFFFF;
		border-radius: 16rpx;
		padding: 20rpx 30rpx;
		margin-bottom: 20rpx;
		box-shadow: 0 2rpx 8rpx rgba(0,0,0,0.03);
	}

	.card-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding-bottom: 20rpx;
	}

	.card-type {
		font-size: 36rpx; /* 标题字号稍大 */
		font-weight: bold;
		position: relative;
		padding-left: 0;
	}

	.detail-link {
		font-size: 26rpx;
		color: #666;
		display: flex;
		align-items: center;
	}
	
	.arrow {
		margin-left: 6rpx;
		font-family: monospace;
	}

	.divider {
		height: 1rpx;
		background-color: #F0F0F0;
		border-bottom: 1px dashed #E5E5E5;
		margin-bottom: 20rpx;
	}

	.card-body {
		display: flex;
		flex-direction: column;
	}

	.info-row {
		display: flex;
		margin-bottom: 16rpx;
		font-size: 28rpx;
	}
	
	.info-row:last-child {
		margin-bottom: 0;
	}

	.label {
		color: #666;
		width: 200rpx; /* 标签宽度稍宽以容纳“保函金额 (元)” */
		flex-shrink: 0;
	}

	.value {
		color: #333;
		flex: 1;
		text-align: right;
	}

	/* 悬浮按钮 */
	.fab-button {
		position: fixed;
		right: 40rpx;
		bottom: 140rpx; /* 避开 TabBar */
		width: 100rpx;
		height: 100rpx;
		background-color: #9D9075; /* 类似图片中的土金色 */
		border-radius: 50%;
		display: flex;
		justify-content: center;
		align-items: center;
		box-shadow: 0 4rpx 10rpx rgba(0,0,0,0.2);
		z-index: 999;
	}

	.plus-icon {
		color: #FFFFFF;
		font-size: 60rpx;
		font-weight: lighter;
		line-height: 1;
		margin-top: -6rpx;
	}
</style>
