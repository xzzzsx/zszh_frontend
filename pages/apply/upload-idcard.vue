<template>
	<view class="container">
		<view class="header-tips">*请上传/拍摄您的身份证照片</view>

		<!-- 身份证正反面上传区 -->
		<view class="id-card-upload-box">
			<view class="upload-item" @click="chooseImage('front')">
				<view class="image-wrapper" v-if="!frontImg">
					<view class="placeholder-icon user-icon"></view>
					<view class="upload-label">上传身份证人像面照片</view>
					<!-- 简单的模拟图标样式 -->
					<view class="scan-corners"></view>
				</view>
				<image v-else :src="frontImg" mode="aspectFit" class="preview-img"></image>
			</view>
			
			<view class="upload-item" @click="chooseImage('back')">
				<view class="image-wrapper" v-if="!backImg">
					<view class="placeholder-icon emblem-icon"></view>
					<view class="upload-label">上传身份证国徽面照片</view>
					<view class="scan-corners"></view>
				</view>
				<image v-else :src="backImg" mode="aspectFit" class="preview-img"></image>
			</view>
		</view>

		<!-- 表单区域 -->
		<view class="form-section">
			<view class="form-row">
				<text class="required-mark">*</text>
				<text class="label">证件类型</text>
				<radio-group @change="onTypeChange" class="radio-group">
					<label class="radio-item">
						<radio value="mainland" :checked="docType === 'mainland'" color="#007AFF" style="transform:scale(0.8)" />
						<text>大陆居民身份证</text>
					</label>
					<label class="radio-item">
						<radio value="passport" :checked="docType === 'passport'" color="#007AFF" style="transform:scale(0.8)" />
						<text>护照</text>
					</label>
					<label class="radio-item last-radio">
						<radio value="permit" :checked="docType === 'permit'" color="#007AFF" style="transform:scale(0.8)" />
						<text>港澳台通行证</text>
					</label>
				</radio-group>
			</view>
			<view class="divider"></view>

			<view class="form-row">
				<text class="required-mark">*</text>
				<text class="label">姓名</text>
				<input class="input-field" v-model="name" placeholder="上传识别后自动填充" disabled />
			</view>
			<view class="divider"></view>

			<view class="form-row">
				<text class="required-mark">*</text>
				<text class="label">证件号码</text>
				<input class="input-field" v-model="idNumber" placeholder="上传识别后自动填充" disabled />
			</view>
			<view class="divider"></view>
		</view>

		<!-- 底部按钮 -->
		<view class="footer">
			<button class="btn-cancel" @click="goBack">取消</button>
			<button class="btn-confirm" @click="confirmUpload">确定</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				frontImg: '',
				backImg: '',
				docType: 'mainland',
				name: '',
				idNumber: ''
			}
		},
		onLoad() {
			uni.setNavigationBarTitle({
				title: '上传身份证'
			});
		},
		methods: {
			chooseImage(side) {
				uni.chooseImage({
					count: 1,
					success: (res) => {
						if (side === 'front') {
							this.frontImg = res.tempFilePaths[0];
							// 模拟OCR识别
							setTimeout(() => {
								this.name = '张三';
								this.idNumber = '110101199001011234';
								uni.showToast({
									title: '识别成功',
									icon: 'none'
								});
							}, 1000);
						} else {
							this.backImg = res.tempFilePaths[0];
						}
					}
				});
			},
			onTypeChange(e) {
				this.docType = e.detail.value;
			},
			goBack() {
				uni.navigateBack();
			},
			confirmUpload() {
				if (!this.frontImg || !this.backImg) {
					uni.showToast({
						title: '请上传正反面照片',
						icon: 'none'
					});
					return;
				}
				
				const pages = getCurrentPages();
				const prevPage = pages[pages.length - 2];
				
				if (prevPage && prevPage.$vm.handleUploadSuccess) {
					prevPage.$vm.handleUploadSuccess('法人身份证', {
						front: this.frontImg,
						back: this.backImg,
						name: this.name,
						idNumber: this.idNumber
					});
				}
				
				uni.navigateBack();
			}
		}
	}
</script>

<style>
	page {
		background-color: #FFFFFF;
	}

	.container {
		padding: 30rpx;
		padding-bottom: 180rpx;
	}

	.header-tips {
		color: #FF4D4F;
		font-size: 28rpx;
		margin-bottom: 30rpx;
	}

	.id-card-upload-box {
		display: flex;
		justify-content: space-between;
		margin-bottom: 50rpx;
	}

	.upload-item {
		width: 48%;
		height: 220rpx;
		border: 1rpx dashed #C8A068;
		border-radius: 12rpx;
		background-color: #F9F9F9;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		position: relative;
		overflow: hidden;
	}

	.image-wrapper {
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.preview-img {
		width: 100%;
		height: 100%;
	}

	.placeholder-icon {
		width: 80rpx;
		height: 50rpx;
		background-color: #E0E0E0;
		margin-bottom: 20rpx;
		border-radius: 4rpx;
	}
	
	.user-icon::after {
		content: '';
		display: block;
		width: 30rpx;
		height: 30rpx;
		background-color: #999;
		border-radius: 50%;
		margin: 10rpx auto;
	}

	.emblem-icon::after {
		content: '';
		display: block;
		width: 30rpx;
		height: 20rpx;
		border: 2rpx solid #999;
		margin: 15rpx auto;
	}

	.upload-label {
		font-size: 22rpx;
		color: #007AFF;
		background-color: #E6F1FF;
		width: 100%;
		text-align: center;
		padding: 10rpx 0;
		position: absolute;
		bottom: 0;
	}

	/* 表单样式 */
	.form-section {
		margin-top: 20rpx;
	}

	.form-row {
		display: flex;
		align-items: flex-start;
		padding: 30rpx 0;
		flex-wrap: wrap;
	}

	.required-mark {
		color: #FF4D4F;
		margin-right: 4rpx;
		margin-top: 6rpx;
	}

	.label {
		font-size: 30rpx;
		color: #333;
		width: 140rpx;
		margin-top: 6rpx;
	}

	.radio-group {
		flex: 1;
		display: flex;
		flex-wrap: wrap;
	}

	.radio-item {
		margin-right: 30rpx;
		margin-bottom: 10rpx;
		font-size: 28rpx;
		color: #333;
		display: flex;
		align-items: center;
	}

	.input-field {
		flex: 1;
		font-size: 30rpx;
		color: #333;
	}

	.divider {
		height: 1rpx;
		background-color: #F0F0F0;
		width: 100%;
	}

	/* 底部按钮 */
	.footer {
		position: fixed;
		bottom: 0;
		left: 0;
		width: 100%;
		padding: 20rpx 30rpx 50rpx;
		background-color: #FFFFFF;
		display: flex;
		justify-content: space-between;
		box-shadow: 0 -2rpx 10rpx rgba(0,0,0,0.05);
	}

	.footer button {
		width: 45%;
		height: 88rpx;
		line-height: 88rpx;
		font-size: 32rpx;
		border-radius: 44rpx;
	}

	.btn-cancel {
		background-color: #FFFFFF;
		color: #999;
		border: 1rpx solid #EEE;
	}
	
	.btn-cancel::after {
		border: none;
	}

	.btn-confirm {
		background-color: #A9906A;
		color: #FFFFFF;
	}
	
	.btn-confirm::after {
		border: none;
	}
</style>
