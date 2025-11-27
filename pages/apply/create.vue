<template>
	<view class="container">
		<view class="tips">请填写以下信息申请存储通知书</view>
		
		<!-- 表单信息 -->
		<view class="card">
			<view class="form-item">
				<text class="label">项目名称</text>
				<input class="input-value" v-model="formData.projectName" placeholder="请输入" placeholder-class="placeholder" />
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">合同开始日期</text>
				<picker mode="date" :value="formData.startDate" @change="onStartDateChange">
					<view class="picker-display">
						<text :class="['value', formData.startDate ? '' : 'placeholder']">{{ formData.startDate || '年 / 月 / 日' }}</text>
						<text class="icon-calendar">&#128197;</text>
					</view>
				</picker>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">合同到期日期</text>
				<picker mode="date" :value="formData.endDate" @change="onEndDateChange">
					<view class="picker-display">
						<text :class="['value', formData.endDate ? '' : 'placeholder']">{{ formData.endDate || '年 / 月 / 日' }}</text>
						<text class="icon-calendar">&#128197;</text>
					</view>
				</picker>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">中标合同价</text>
				<input class="input-value" type="digit" v-model="formData.contractPrice" placeholder="请输入" placeholder-class="placeholder" />
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">保证金缴纳类型</text>
				<picker mode="selector" :range="typeOptions" :value="typeIndex" @change="onTypeChange">
					<view class="picker-display">
						<text :class="['value', formData.guaranteeType ? '' : 'placeholder']">{{ formData.guaranteeType || '请选择' }}</text>
						<text class="arrow-down">∨</text>
					</view>
				</picker>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">联系人姓名</text>
				<input class="input-value" v-model="formData.contactName" placeholder="请输入" placeholder-class="placeholder" />
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">联系人电话</text>
				<input class="input-value" type="number" v-model="formData.contactPhone" placeholder="请输入" placeholder-class="placeholder" />
			</view>
			<view class="divider"></view>
			
			<view class="form-item address-item">
				<text class="label">工程地址</text>
				<view class="address-wrapper">
					<picker mode="selector" :range="regionOptions" :value="regionIndex" @change="onRegionChange">
						<view class="picker-display">
							<text :class="['value', formData.region ? '' : 'placeholder']">{{ formData.region || '请选择' }}</text>
							<text class="arrow-down">∨</text>
						</view>
					</picker>
					<input class="address-input" v-model="formData.addressDetail" placeholder="请输入详细地址" placeholder-class="placeholder" />
				</view>
			</view>
		</view>

		<view class="tips">请上传申请资料</view>
		<!-- 申请资料 -->
		<view class="card">
			<view class="form-item" v-for="(file, index) in fileList" :key="index">
				<text class="label">{{ file.name }}</text>
				<text class="upload-link" @click="uploadFile(file)">{{ file.uploaded ? '已上传' : '去上传 >' }}</text>
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
					projectName: '',
					startDate: '',
					endDate: '',
					contractPrice: '',
					guaranteeType: '',
					contactName: '',
					contactPhone: '',
					region: '',
					addressDetail: ''
				},
				typeOptions: ['保险保单', '现金存单', '银行保函'],
				typeIndex: 0,
				regionOptions: [
					'辽宁省-丹东市-元宝区',
					'辽宁省-丹东市-振兴区',
					'辽宁省-丹东市-振安区',
					'辽宁省-丹东市-宽甸满族自治县',
					'辽宁省-丹东市-东港市',
					'辽宁省-丹东市-凤城市'
				],
				regionIndex: 0,
				fileList: [
					{ name: '施工合同', uploaded: false },
					{ name: '营业执照', uploaded: false },
					{ name: '法人身份证', uploaded: false },
					{ name: '中标通知书', uploaded: false }
				]
			}
		},
		methods: {
			onStartDateChange(e) {
				this.formData.startDate = e.detail.value;
			},
			onEndDateChange(e) {
				this.formData.endDate = e.detail.value;
			},
			onTypeChange(e) {
				this.typeIndex = Number(e.detail.value);
				this.formData.guaranteeType = this.typeOptions[this.typeIndex];
			},
			onRegionChange(e) {
				this.regionIndex = Number(e.detail.value);
				this.formData.region = this.regionOptions[this.regionIndex];
			},
			uploadFile(file) {
				if (file.name === '营业执照') {
					uni.navigateTo({
						url: '/pages/apply/upload?title=' + file.name
					});
				} else if (file.name === '法人身份证') {
					uni.navigateTo({
						url: '/pages/apply/upload-idcard'
					});
				} else {
					uni.chooseImage({
						count: 1,
						success: (res) => {
							file.uploaded = true;
							uni.showToast({ title: '上传成功', icon: 'success' });
						}
					});
				}
			},
			handleUploadSuccess(title, data) {
				// 查找对应的文件项并标记为已上传
				const file = this.fileList.find(item => item.name === title);
				if (file) {
					file.uploaded = true;
					// 如果是身份证，data 是一个包含正反面和信息的对象
					if (title === '法人身份证') {
						console.log('身份证上传成功:', data);
						// 这里可以将身份证信息保存到 formData 中
						// this.formData.idCardInfo = data;
					} else {
						// 普通文件，data 是 filePath
						// file.path = data;
					}
				}
			},
			onCancel() {
				uni.navigateBack();
			},
			onSubmit() {
				uni.showToast({ title: '提交成功', icon: 'success' });
				setTimeout(() => {
					uni.navigateBack();
				}, 1500);
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

	.tips {
		font-size: 28rpx;
		color: #999;
		margin: 20rpx 10rpx;
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
		color: #333;
		font-size: 30rpx;
		flex-shrink: 0;
		width: 240rpx;
	}

	.value {
		color: #333;
		font-size: 30rpx;
		text-align: right;
		flex: 1;
	}

	.input-value {
		flex: 1;
		text-align: right;
		font-size: 30rpx;
		color: #333;
	}

	.placeholder {
		color: #CCC;
	}

	.picker-display {
		display: flex;
		align-items: center;
		justify-content: flex-end;
		flex: 1;
	}

	.icon-calendar {
		margin-left: 10rpx;
		font-size: 32rpx;
	}
	
	.arrow-down {
		margin-left: 10rpx;
		color: #999;
		font-weight: bold;
	}

	.divider {
		height: 1rpx;
		background-color: #F0F0F0;
		width: 100%;
	}

	.address-item {
		align-items: flex-start;
	}

	.address-wrapper {
		flex: 1;
		display: flex;
		flex-direction: column;
		align-items: flex-end;
	}

	.address-input {
		width: 100%;
		text-align: right;
		font-size: 30rpx;
		color: #333;
		margin-top: 20rpx;
	}

	.upload-link {
		color: #999;
		font-size: 28rpx;
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
		background-color: #999;
		color: #FFFFFF;
		border: none;
	}

	.btn-submit {
		background-color: #8FBC8F; /* 类似的绿色 */
		color: #FFFFFF;
		border: none;
	}
	
	.btn-cancel::after, .btn-submit::after {
		border: none;
	}
</style>
