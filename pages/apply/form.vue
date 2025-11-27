<template>
	<view class="container">
		<!-- 保函信息 -->
		<view class="section-title">保函信息</view>
		<view class="card">
			<view class="form-item">
				<text class="label">合同开始日期</text>
				<picker mode="date" :value="formData.startDate" @change="onStartDateChange">
					<view class="picker-display">
						<text :class="['value', formData.startDate ? '' : 'placeholder']">{{ formData.startDate || '年 / 月 / 日' }}</text>
						<text class="icon">📅</text>
					</view>
				</picker>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">合同到期日期</text>
				<picker mode="date" :value="formData.endDate" @change="onEndDateChange">
					<view class="picker-display">
						<text :class="['value', formData.endDate ? '' : 'placeholder']">{{ formData.endDate || '年 / 月 / 日' }}</text>
						<text class="icon">📅</text>
					</view>
				</picker>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">是否需要开发票</text>
				<picker mode="selector" :range="invoiceOptions" :value="invoiceIndex" @change="onInvoiceChange">
					<view class="picker-display">
						<text class="value">{{ formData.needInvoice }}</text>
						<text class="icon">∨</text>
					</view>
				</picker>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">保证金金额（元）</text>
				<input class="input-value" type="digit" v-model="formData.amount" placeholder="请输入" placeholder-class="placeholder" />
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">保证金存储通知书</text>
				<text class="file-value">保证金存储通知书.pdf</text>
			</view>
		</view>

		<!-- 被保证人信息 -->
		<view class="section-title">被保证人信息</view>
		<view class="card">
			<view class="form-item">
				<text class="label">被保证人名称</text>
				<text class="value">深圳致和易保科技有限公司</text>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">被保证人联系人</text>
				<text class="value">林大大</text>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">被保证人联系人电话</text>
				<text class="value">18954655511</text>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">被保证人法人名称</text>
				<text class="value">胡伟雄</text>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">被保证人注册地址</text>
				<text class="value">深圳市福田区沙头街</text>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">统一社会信用代码</text>
				<input class="input-value" v-model="formData.creditCode" placeholder="请输入" placeholder-class="placeholder" />
			</view>
		</view>

		<!-- 请上传申请资料 -->
		<view class="section-title">请上传申请资料</view>
		<view class="card">
			<view class="form-item" v-for="(file, index) in fileList" :key="index">
				<text class="label">{{ file.name }}</text>
				<text class="file-value" v-if="file.uploaded" @click="previewFile(file)">{{ file.fileName }}</text>
				<text class="upload-link" v-else @click="uploadFile(file)">去上传</text>
			</view>
			<view class="divider" v-for="(file, index) in fileList.slice(0, -1)" :key="'d'+index"></view>
		</view>

		<!-- 底部按钮 -->
		<view class="bottom-buttons">
			<button class="btn-cancel" @click="onCancel">取消</button>
			<button class="btn-submit" @click="onSubmit">提交</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				formData: {
					startDate: '',
					endDate: '',
					needInvoice: '否',
					amount: '',
					creditCode: ''
				},
				invoiceOptions: ['否', '是'],
				invoiceIndex: 0,
				fileList: [
					{ name: '营业执照', fileName: '营业执照.png', uploaded: true },
					{ name: '资质证明', fileName: '资质证明.png', uploaded: true },
					{ name: '法人身份证', fileName: '法人身份证国徽面.png、人像面.png', uploaded: true },
					{ name: '近两年审计报告', fileName: '', uploaded: false },
					{ name: '中标通知书', fileName: '中标通知书.pdf', uploaded: true },
					{ name: '保险保函申请书', fileName: '', uploaded: false },
					{ name: '施工总承包单位承诺书', fileName: '', uploaded: false }
				]
			}
		},
		onLoad(options) {
			// 可以接收传递的参数
		},
		methods: {
			onStartDateChange(e) {
				this.formData.startDate = e.detail.value;
			},
			onEndDateChange(e) {
				this.formData.endDate = e.detail.value;
			},
			onInvoiceChange(e) {
				this.invoiceIndex = Number(e.detail.value);
				this.formData.needInvoice = this.invoiceOptions[this.invoiceIndex];
			},
			uploadFile(file) {
				uni.chooseImage({
					count: 1,
					success: (res) => {
						file.uploaded = true;
						file.fileName = res.tempFilePaths[0].split('/').pop();
						uni.showToast({ title: '上传成功', icon: 'success' });
					}
				});
			},
			previewFile(file) {
				uni.showToast({ title: '预览: ' + file.fileName, icon: 'none' });
			},
			onCancel() {
				uni.navigateBack();
			},
			onSubmit() {
				uni.showModal({
					title: '提示',
					content: '确定提交申请吗？',
					success: (res) => {
						if (res.confirm) {
							uni.showToast({ title: '提交成功', icon: 'success' });
							setTimeout(() => {
								uni.navigateBack();
							}, 1500);
						}
					}
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
		padding: 20rpx;
		padding-bottom: 180rpx;
	}

	.section-title {
		font-size: 32rpx;
		font-weight: bold;
		color: #C8A068;
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
		color: #666;
		font-size: 28rpx;
		flex-shrink: 0;
		max-width: 280rpx;
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

	.input-value {
		flex: 1;
		text-align: right;
		font-size: 28rpx;
		color: #333;
	}

	.file-value {
		color: #333;
		font-size: 28rpx;
		text-align: right;
		flex: 1;
	}

	.upload-link {
		color: #C8A068;
		font-size: 28rpx;
	}

	.divider {
		height: 1rpx;
		background-color: #F0F0F0;
		width: 100%;
	}

	/* 底部按钮 */
	.bottom-buttons {
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;
		display: flex;
		padding: 20rpx 30rpx;
		padding-bottom: 40rpx;
		background-color: #FFFFFF;
		box-shadow: 0 -2rpx 10rpx rgba(0,0,0,0.05);
	}

	.btn-cancel, .btn-submit {
		flex: 1;
		height: 88rpx;
		line-height: 88rpx;
		font-size: 32rpx;
		border-radius: 44rpx;
		margin: 0 15rpx;
	}

	.btn-cancel {
		background-color: #FFFFFF;
		color: #C8A068;
		border: 2rpx solid #C8A068;
	}

	.btn-submit {
		background-color: #C8A068;
		color: #FFFFFF;
		border: none;
	}

	.btn-cancel::after, .btn-submit::after {
		border: none;
	}
</style>
