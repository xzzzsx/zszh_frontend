<template>
	<view class="container">
		<!-- 项目资料 -->
		<view class="section-title">项目资料</view>
		<view class="card">
			<view class="form-item">
				<text class="label">保函开立时间</text>
				<picker mode="date" :value="formData.issueDate" @change="onIssueDateChange" :disabled="!editing">
					<view class="picker-display">
						<text :class="['value', formData.issueDate ? '' : 'placeholder']">{{ formData.issueDate || '年 / 月 / 日' }}</text>
						<text class="icon">📅</text>
					</view>
				</picker>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">保函文件</text>
				<text class="file-value">G228XXXX1.pdf</text>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">申请单编号</text>
				<picker mode="date" :value="formData.applyNo" @change="onApplyNoChange" :disabled="!editing">
					<view class="picker-display">
						<text :class="['value', formData.applyNo ? '' : 'placeholder']">{{ formData.applyNo || '年 / 月 / 日' }}</text>
						<text class="icon">📅</text>
					</view>
				</picker>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">保函申请日期</text>
				<text class="value">{{ formData.applyDate }}</text>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">开票状态</text>
				<picker mode="selector" :range="invoiceStatusOptions" :value="invoiceStatusIndex" @change="onInvoiceStatusChange" :disabled="!editing">
					<view class="picker-display">
						<text class="value">{{ formData.invoiceStatus }}</text>
						<text class="icon">∨</text>
					</view>
				</picker>
			</view>
		</view>

		<!-- 申请资料 -->
		<view class="section-title">申请资料</view>
		<view class="card">
			<view class="file-item" v-for="(file, index) in fileList" :key="index">
				<text class="label">{{ file.name }}</text>
				<text class="file-value" @click="previewFile(file)">{{ file.fileName }}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				editing: true,
				formData: {
					issueDate: '',
					applyNo: '',
					applyDate: '2025-07-28',
					invoiceStatus: '待申请开'
				},
				invoiceStatusOptions: ['待申请开票', '已申请开票', '已开票'],
				invoiceStatusIndex: 0,
				fileList: [
					{ name: '营业执照', fileName: '营业执照.pdf' },
					{ name: '施工合同', fileName: '施工合同.pdf' },
					{ name: '法人身份证', fileName: '法人身份证.png' },
					{ name: '中标通知书', fileName: '中标通知书.pdf' }
				]
			}
		},
		onLoad(options) {
			// 可以接收传递的参数
		},
		methods: {
			onIssueDateChange(e) {
				this.formData.issueDate = e.detail.value;
			},
			onApplyNoChange(e) {
				this.formData.applyNo = e.detail.value;
			},
			onInvoiceStatusChange(e) {
				this.invoiceStatusIndex = Number(e.detail.value);
				this.formData.invoiceStatus = this.invoiceStatusOptions[this.invoiceStatusIndex];
			},
			previewFile(file) {
				uni.showToast({ title: '预览: ' + file.fileName, icon: 'none' });
			}
		}
	}
</script>

<style>
	page {
		background-color: #F8F8F8;
	}
	
	.container {
		padding: 20rpx;
		padding-bottom: 50rpx;
	}

	.section-title {
		font-size: 32rpx;
		font-weight: bold;
		color: #333;
		margin-bottom: 20rpx;
		padding-left: 10rpx;
	}

	.card {
		background-color: #FFFFFF;
		border-radius: 16rpx;
		padding: 0 30rpx;
		margin-bottom: 30rpx;
		box-shadow: 0 2rpx 8rpx rgba(0,0,0,0.03);
	}

	.form-item {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 30rpx 0;
		min-height: 40rpx;
	}

	.label {
		color: #333333;
		font-size: 28rpx;
		flex-shrink: 0;
	}

	.value {
		color: #333;
		font-size: 28rpx;
		text-align: right;
		flex: 1;
	}

	.placeholder {
		color: #CCC;
	}

	.picker-display {
		display: flex;
		align-items: center;
		justify-content: flex-end;
	}

	.icon {
		margin-left: 10rpx;
		color: #999;
	}

	.file-value {
		color: #333;
		font-size: 28rpx;
		text-align: right;
		flex: 1;
	}

	.divider {
		height: 1rpx;
		background-color: #F0F0F0;
		width: 100%;
	}

	.file-item {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 30rpx 0;
		border-bottom: 1rpx solid #F0F0F0;
	}

	.file-item:last-child {
		border-bottom: none;
	}
</style>
