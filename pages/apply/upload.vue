<template>
	<view class="container">
		<view class="upload-card">
			<view class="card-header">
				<text class="title">{{ title }}</text>
				<text class="desc">仅支持上传1个文件，单文件小于15MB</text>
				<text class="desc">支持文件格式：jpg/jpeg/png</text>
			</view>
			<view class="upload-area" @click="chooseImage">
				<view class="upload-btn" v-if="!tempFilePath">
					<text class="plus-icon">+</text>
					<text class="upload-text">请拍照或上传添加文件</text>
				</view>
				<image v-else :src="tempFilePath" mode="aspectFit" class="preview-image"></image>
				<view v-if="tempFilePath" class="re-upload" @click.stop="chooseImage">重新上传</view>
			</view>
		</view>

		<view class="ocr-check">
			<checkbox-group @change="onOcrChange">
				<label class="checkbox-label">
					<checkbox value="ocr" :checked="ocrEnabled" color="#C8A068" style="transform:scale(0.7)" />
					<text>是否启用OCR识别</text>
				</label>
			</checkbox-group>
		</view>

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
				title: '营业执照', // 默认为营业执照，可动态传参
				ocrEnabled: true,
				tempFilePath: ''
			}
		},
		onLoad(options) {
			if (options.title) {
				this.title = options.title;
			}
			uni.setNavigationBarTitle({
				title: '上传文件'
			});
		},
		methods: {
			chooseImage() {
				uni.chooseImage({
					count: 1,
					sizeType: ['original', 'compressed'],
					sourceType: ['album', 'camera'],
					success: (res) => {
						// tempFilePath可以作为img标签的src属性显示图片
						this.tempFilePath = res.tempFilePaths[0];
					}
				});
			},
			onOcrChange(e) {
				this.ocrEnabled = e.detail.value.includes('ocr');
			},
			goBack() {
				uni.navigateBack();
			},
			confirmUpload() {
				if (!this.tempFilePath) {
					uni.showToast({
						title: '请先选择文件',
						icon: 'none'
					});
					return;
				}
				// 这里可以处理上传逻辑，或者把临时路径传回上一个页面
				// 模拟上传成功
				const pages = getCurrentPages();
				const prevPage = pages[pages.length - 2]; // 上一个页面
				
				// 如果上一个页面有 handleUploadSuccess 方法，则调用
				if (prevPage && prevPage.$vm.handleUploadSuccess) {
					prevPage.$vm.handleUploadSuccess(this.title, this.tempFilePath);
				}
				
				uni.navigateBack();
			}
		}
	}
</script>

<style>
	page {
		background-color: #F8F8F8;
	}

	.container {
		padding: 30rpx;
	}

	.upload-card {
		background-color: #FFFFFF;
		border-radius: 16rpx;
		padding: 30rpx;
		margin-bottom: 30rpx;
	}

	.card-header {
		margin-bottom: 30rpx;
	}

	.title {
		font-size: 32rpx;
		font-weight: bold;
		color: #333;
		display: block;
		margin-bottom: 10rpx;
	}

	.desc {
		font-size: 24rpx;
		color: #999;
		display: block;
		line-height: 1.5;
	}

	.upload-area {
		background-color: #FFFFFF;
		border-top: 1rpx solid #EEE;
		padding-top: 30rpx;
		min-height: 100rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.upload-btn {
		display: flex;
		align-items: center;
		justify-content: flex-start; /* 左对齐 */
		width: 100%;
	}

	.plus-icon {
		background-color: #0052D9; /* 蓝色背景 */
		color: #FFFFFF;
		width: 40rpx;
		height: 40rpx;
		border-radius: 50%;
		text-align: center;
		line-height: 36rpx;
		font-size: 32rpx;
		margin-right: 16rpx;
		font-weight: bold;
	}

	.upload-text {
		color: #0052D9; /* 蓝色文字 */
		font-size: 30rpx;
	}

	.preview-image {
		width: 100%;
		height: 400rpx;
		background-color: #F5F5F5;
		border-radius: 8rpx;
	}
	
	.re-upload {
		margin-top: 20rpx;
		color: #0052D9;
		font-size: 28rpx;
		text-align: center;
	}

	.ocr-check {
		margin-top: 30rpx;
		padding-left: 10rpx;
	}
	
	.checkbox-label {
		display: flex;
		align-items: center;
		font-size: 28rpx;
		color: #666;
	}

	.footer {
		position: fixed;
		bottom: 0;
		left: 0;
		width: 100%;
		padding: 20rpx 30rpx 50rpx; /* 底部留白适配全面屏 */
		background-color: #FFFFFF;
		display: flex;
		justify-content: space-between;
		box-sizing: border-box;
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
		background-color: #A9906A; /* 金色背景 */
		color: #FFFFFF;
	}
	
	.btn-confirm::after {
		border: none;
	}
</style>
