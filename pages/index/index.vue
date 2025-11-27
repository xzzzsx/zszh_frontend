<template>
	<view class="container">
		<!-- 通知栏 -->
		<view class="notice-bar">
			<text class="notice-icon">📢</text>
			<text class="notice-text">暂无公告</text>
		</view>

		<!-- 状态统计卡片 -->
		<view class="stats-container">
			<view class="stat-card">
				<text class="stat-title">待支付</text>
				<text class="stat-count">0</text>
			</view>
			<view class="stat-card">
				<text class="stat-title">待出函</text>
				<text class="stat-count">0</text>
			</view>
			<view class="stat-card">
				<text class="stat-title">已出函</text>
				<text class="stat-count">0</text>
			</view>
		</view>

		<!-- 快捷操作区域 -->
		<view class="section-container">
			<view class="section-header">
				<text class="section-title">快捷操作</text>
			</view>
			<view class="grid-container">
				<view class="grid-item" v-for="(item, index) in quickActions" :key="index">
					<view class="icon-box">
						<!-- 这里后续可以替换为 <image src="..." /> -->
						<view class="placeholder-icon">📄</view>
					</view>
					<text class="grid-text">{{ item.name }}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				quickActions: [
					{ name: '申请存储通知书', icon: '' },
					{ name: '申请保函', icon: '' },
					{ name: '续开保函', icon: '' },
					{ name: '上传凭证', icon: '' },
					{ name: '申请返还', icon: '' }
				]
			}
		},
		methods: {

		}
	}
</script>

<style>
	.container {
		padding: 0;
		background-color: #FFFFFF;
		min-height: 100vh;
	}

	/* 通知栏 */
	.notice-bar {
		display: flex;
		align-items: center;
		background-color: #FFFBE8;
		padding: 20rpx 30rpx;
		color: #ED6A0C;
		font-size: 28rpx;
	}

	.notice-icon {
		margin-right: 10rpx;
	}

	/* 状态统计卡片 */
	.stats-container {
		display: flex;
		justify-content: space-between;
		padding: 30rpx;
	}

	.stat-card {
		flex: 1;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		background-color: #FFFFFF;
		border: 1rpx solid #E5E5E5;
		border-radius: 10rpx;
		padding: 40rpx 0;
		margin: 0 10rpx;
		box-shadow: 0 2rpx 6rpx rgba(0, 0, 0, 0.02);
	}

	.stat-card:first-child {
		margin-left: 0;
	}

	.stat-card:last-child {
		margin-right: 0;
	}

	.stat-title {
		font-size: 30rpx;
		color: #333333;
		margin-bottom: 10rpx;
	}

	.stat-count {
		font-size: 36rpx;
		color: #333333;
		font-weight: bold;
	}

	/* 快捷操作区域 */
	.section-container {
		margin: 20rpx 30rpx;
		border: 1rpx solid #E5E5E5;
		border-radius: 10rpx;
		padding: 20rpx;
	}

	.section-header {
		margin-bottom: 30rpx;
		padding-left: 10rpx;
	}

	.section-title {
		font-size: 32rpx;
		font-weight: bold;
		color: #333333;
	}

	.grid-container {
		display: flex;
		flex-wrap: wrap;
	}

	.grid-item {
		width: 33.33%; /* 三列布局 */
		display: flex;
		flex-direction: column;
		align-items: center;
		margin-bottom: 40rpx;
	}

	.icon-box {
		width: 100rpx;
		height: 100rpx;
		background-color: #F2F2F2;
		border-radius: 10rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		margin-bottom: 16rpx;
	}

	.placeholder-icon {
		font-size: 50rpx;
	}

	.grid-text {
		font-size: 26rpx;
		color: #333333;
		text-align: center;
		width: 90%;
	}
</style>
