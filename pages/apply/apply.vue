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
				<view class="card-header" :class="{ 'header-right': !item.type }">
					<!-- 状态标题带颜色 -->
					<text class="card-type" v-if="item.type" :style="{ color: item.titleColor || '#333' }">{{ item.type }}</text>
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
					<!-- 根据 Tab 显示不同字段 -->
					<view class="info-row" v-if="currentTab === 0">
						<text class="label">申请状态：</text>
						<text class="value status-text" :class="item.statusClass">{{ item.status }}</text>
					</view>
					<view class="info-row" v-else>
						<text class="label">保证金金额：</text>
						<text class="value">{{ item.amount }}</text>
					</view>
				</view>
				<!-- 操作按钮 -->
				<view class="card-action" v-if="item.actionText">
					<button class="action-btn" :class="item.actionClass" @click="handleAction(item)">{{ item.actionText }}</button>
				</view>
			</view>
		</view>

		<!-- 悬浮按钮 -->
		<view class="fab-button" @click="goToCreate">
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
				tabs: ['申请存储通知书', '申请保函', '续开保函', '上传现金凭证', '申请返还'],
				// Tab 0: 申请存储通知书
				list0: [
					{
						type: '保险保单',
						projectName: '科创中心实验成果转化基地施工项目',
						institution: '大家财产保险有限责任公司',
						status: '申请通过',
						statusClass: 'success'
					},
					{
						type: '现金存单',
						projectName: '科创中心实验成果转化基地施工项目',
						institution: '建设银行辽宁丹东支行',
						status: '审核中',
						statusClass: 'warning'
					},
					{
						type: '银行保函',
						projectName: '科创中心实验成果转化基地施工项目',
						institution: '建设银行辽宁丹东支行',
						status: '申请通过',
						statusClass: 'success'
					}
				],
				// Tab 1: 申请保函
				list1: [
					{
						type: '已出函',
						titleColor: '#5AC725',
						projectName: '卧龙山至太平湾南段路面改造工程项目',
						institution: '大家财产保险有限责任公司',
						amount: '2,000,000.00',
						actionText: '下载保函',
						actionClass: 'btn-primary'
					},
					{
						type: '待审核',
						titleColor: '#FF9900',
						projectName: '甘井子区丹东线棋盘子桥改建工程',
						institution: '中国人寿财产保险股份有限公司',
						amount: '660,000.00',
						actionText: '',
						actionClass: ''
					},
					{
						type: '未申请',
						titleColor: '#999999',
						projectName: '丹东市妇幼保健计划生育服务中心1号门诊',
						institution: '永诚财产保险股份有限公司',
						amount: '1,000,000.00',
						actionText: '申请保函',
						actionClass: 'btn-primary'
					}
				],
				// Tab 2: 续开保函
				list2: [
					{
						type: '',
						titleColor: '',
						projectName: '卧龙山至太平湾南段路面改造工程项目',
						institution: '大家财产保险有限责任公司',
						amount: '2,000,000.00',
						actionText: '续开保函',
						actionClass: 'btn-primary'
					},
					{
						type: '',
						titleColor: '',
						projectName: '甘井子区丹东线棋盘子桥改建工程',
						institution: '中国人寿财产保险股份有限公司',
						amount: '660,000.00',
						actionText: '续开保函',
						actionClass: 'btn-primary'
					},
					{
						type: '',
						titleColor: '',
						projectName: '丹东市妇幼保健计划生育服务中心1号门诊',
						institution: '永诚财产保险股份有限公司',
						amount: '1,000,000.00',
						actionText: '续开保函',
						actionClass: 'btn-primary'
					}
				],
				// Tab 3: 上传现金凭证
				list3: [
					{
						type: '',
						titleColor: '',
						projectName: '卧龙山至太平湾南段路面改造工程项目',
						institution: '中国工商银行',
						amount: '2,000,000.00',
						actionText: '上传凭证',
						actionClass: 'btn-primary'
					},
					{
						type: '',
						titleColor: '',
						projectName: '甘井子区丹东线棋盘子桥改建工程',
						institution: '中国人寿财产保险股份有限公司',
						amount: '660,000.00',
						actionText: '上传凭证',
						actionClass: 'btn-primary'
					},
					{
						type: '',
						titleColor: '',
						projectName: '丹东市妇幼保健计划生育服务中心1号门诊',
						institution: '永诚财产保险股份有限公司',
						amount: '1,000,000.00',
						actionText: '上传凭证',
						actionClass: 'btn-primary'
					}
				],
				// Tab 4: 申请返还
				list4: [
					{
						type: '',
						titleColor: '',
						projectName: '卧龙山至太平湾南段路面改造工程项目',
						institution: '大家财产保险有限责任公司',
						amount: '2,000,000.00',
						actionText: '申请返还',
						actionClass: 'btn-primary'
					},
					{
						type: '',
						titleColor: '',
						projectName: '甘井子区丹东线棋盘子桥改建工程',
						institution: '中国人寿财产保险股份有限公司',
						amount: '660,000.00',
						actionText: '申请返还',
						actionClass: 'btn-primary'
					},
					{
						type: '',
						titleColor: '',
						projectName: '丹东市妇幼保健计划生育服务中心1号门诊',
						institution: '永诚财产保险股份有限公司',
						amount: '1,000,000.00',
						actionText: '申请返还',
						actionClass: 'btn-primary'
					}
				]
			}
		},
		onShow() {
			uni.setNavigationBarTitle({
				title: '申请保证金'
			});
			
			// 检查是否有跳转目标 Tab
			const targetTab = uni.getStorageSync('applyTargetTab');
			if (targetTab !== '' && targetTab !== undefined && targetTab !== null) {
				this.currentTab = Number(targetTab);
				// 清除 storage，防止下次进来还是跳到这个 tab
				uni.removeStorageSync('applyTargetTab');
			}
		},
		computed: {
			currentList() {
				const lists = [this.list0, this.list1, this.list2, this.list3, this.list4];
				return lists[this.currentTab] || [];
			}
		},
		methods: {
			goToDetail(item) {
				uni.navigateTo({
					url: '/pages/apply/detail?id=' + (item.id || 0)
				});
			},
			handleAction(item) {
				if (item.actionText === '下载保函') {
					uni.showToast({ title: '下载中...', icon: 'loading' });
				} else if (item.actionText === '申请保函' || item.actionText === '续开保函') {
					uni.navigateTo({ url: '/pages/apply/form?id=' + (item.id || 0) });
				} else if (item.actionText === '上传凭证') {
					uni.showToast({ title: '上传凭证', icon: 'none' });
				} else if (item.actionText === '申请返还') {
					uni.navigateTo({
						url: '/pages/apply/refund'
					});
				}
			},
			goToCreate() {
				uni.navigateTo({
					url: '/pages/apply/create'
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
		width: 40rpx;
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

	.card-header.header-right {
		justify-content: flex-end;
	}

	.card-type {
		font-size: 32rpx;
		font-weight: bold;
		color: #333;
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
		width: 160rpx;
		flex-shrink: 0;
	}

	.value {
		color: #333;
		flex: 1;
		text-align: right;
	}
	
	.status-text.success {
		color: #5AC725; /* 绿色 */
	}
	
	.status-text.warning {
		color: #FF9900; /* 橙色 */
	}

	/* 卡片操作按钮 */
	.card-action {
		margin-top: 20rpx;
		padding-top: 0;
	}

	.action-btn {
		width: 100%;
		height: 80rpx;
		line-height: 80rpx;
		font-size: 30rpx;
		border-radius: 10rpx;
		border: none;
	}

	.action-btn.btn-primary {
		background-color: #C8A068;
		color: #FFFFFF;
	}

	.action-btn::after {
		border: none;
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
