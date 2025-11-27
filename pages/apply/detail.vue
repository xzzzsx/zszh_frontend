<template>
	<view class="container">
		<!-- 项目资料卡片 -->
		<view class="card">
			<view class="card-header">
				<text class="card-title">项目资料</text>
				<view class="header-btn-group" v-if="!editing">
					<button class="btn-outline" size="mini" @click="startEdit">补充资料</button>
				</view>
				<view class="header-btn-group" v-else>
					<button class="btn-outline" size="mini" @click="cancelEdit">取消</button>
					<button class="btn-primary" size="mini" @click="confirmEdit">确认</button>
				</view>
			</view>
			
			<view class="form-item">
				<text class="label">项目名称</text>
				<text class="value project-name">中心实验成果转化基地施工项目</text>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">合同开始日期</text>
				<view class="value-group">
					<picker mode="date" :value="formData.startDate" @change="onStartDateChange" :disabled="!editing">
						<view class="picker-display" :class="{ disabled: !editing }">
							<text :class="['value', formData.startDate ? '' : 'placeholder']">{{ formData.startDate || '年 / 月 / 日' }}</text>
							<text class="icon-calendar">&#128197;</text>
						</view>
					</picker>
				</view>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">合同到期日期</text>
				<view class="value-group">
					<picker mode="date" :value="formData.endDate" @change="onEndDateChange" :disabled="!editing">
						<view class="picker-display" :class="{ disabled: !editing }">
							<text class="value">{{ formData.endDate }}</text>
							<text class="icon-calendar">&#128197;</text>
						</view>
					</picker>
				</view>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">中标合同价</text>
				<text class="value">20,000,000</text>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">保证金缴纳类型</text>
				<view class="value-group">
					<picker mode="selector" :range="typeOptions" :value="typeIndex" @change="onTypeChange" :disabled="!editing">
						<view class="picker-display" :class="{ disabled: !editing }">
							<text class="value">{{ formData.guaranteeType }}</text>
							<text class="arrow-down">∨</text>
						</view>
					</picker>
				</view>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">联系人姓名</text>
				<text class="value">陈银</text>
			</view>
			<view class="divider"></view>
			
			<view class="form-item">
				<text class="label">联系人电话</text>
				<text class="value">18119122266</text>
			</view>
			<view class="divider"></view>
			
			<view class="form-item address-item">
				<text class="label">工程地址</text>
				<view class="address-wrapper">
					<picker mode="selector" :range="regionOptions" :value="regionIndex" @change="onRegionChange" :disabled="!editing">
						<view class="picker-display" :class="{ disabled: !editing }">
							<text class="value">{{ formData.region }}</text>
							<text class="arrow-down">∨</text>
						</view>
					</picker>
					<input class="address-input" v-model="formData.addressDetail" :disabled="!editing" placeholder="请输入详细地址" />
				</view>
			</view>
		</view>

		<!-- 申请资料卡片 -->
		<view class="section-title">申请资料</view>
		<view class="card">
			<view class="file-item">
				<text class="label">施工合同</text>
				<text class="file-name">施工合同.pdf</text>
			</view>
			<view class="divider"></view>
			
			<view class="file-item">
				<text class="label">营业执照</text>
				<text class="file-name">营业执照.pdf</text>
			</view>
			<view class="divider"></view>
			
			<view class="file-item">
				<text class="label">法人身份证</text>
				<text class="file-name">法人身份证.jpg</text>
			</view>
			<!-- 
			<view class="divider"></view>
			<view class="file-item">
				<text class="label">中标通知书</text>
				<text class="file-name">中标通知书.pdf</text>
			</view> 
			-->
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				editing: true,
				formData: {
					startDate: '',
					endDate: '2026-07-27',
					guaranteeType: '保险保单',
					region: '辽宁省-丹东市-元宝区',
					addressDetail: '东皇大道3号'
				},
				backupData: {},
				typeOptions: ['请选择', '保险保单', '现金存单', '银行保函', '小额项目'],
				typeIndex: 1,
				regionOptions: [
					'辽宁省-丹东市-元宝区',
					'辽宁省-丹东市-振兴区',
					'辽宁省-丹东市-振安区',
					'辽宁省-丹东市-宽甸满族自治县',
					'辽宁省-丹东市-东港市',
					'辽宁省-丹东市-凤城市'
				],
				regionIndex: 0
			}
		},
		onLoad(options) {
			// 这里可以接收上个页面传递的 id
			// console.log(options.id)
		},
		methods: {
			startEdit() {
				this.backupData = JSON.parse(JSON.stringify(this.formData));
				this.editing = true;
			},
			cancelEdit() {
				this.formData = JSON.parse(JSON.stringify(this.backupData));
				this.editing = false;
			},
			confirmEdit() {
				this.editing = false;
				uni.showToast({
					title: '资料已保存',
					icon: 'success'
				});
			},
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

	.card {
		background-color: #FFFFFF;
		border-radius: 16rpx;
		padding: 0 30rpx;
		margin-bottom: 30rpx;
		box-shadow: 0 2rpx 8rpx rgba(0,0,0,0.03);
	}

	.card-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 30rpx 0;
	}

	.header-btn-group {
		display: flex;
		gap: 20rpx;
	}

	.card-title {
		font-size: 34rpx;
		font-weight: bold;
		color: #333;
	}

	.btn-outline {
		background-color: #FFFFFF;
		border: 1rpx solid #C8A068;
		color: #C8A068;
		font-size: 24rpx;
		padding: 0 20rpx;
		line-height: 1.8;
		border-radius: 8rpx;
	}
	
	/* 消除 button 默认样式影响 */
	.btn-outline::after {
		border: none;
	}

	.btn-primary {
		background-color: #C8A068;
		color: #FFFFFF;
		border: 1rpx solid #C8A068;
		font-size: 24rpx;
		padding: 0 30rpx;
		line-height: 1.8;
		border-radius: 8rpx;
	}

	.btn-primary::after {
		border: none;
	}

	.form-item {
		display: flex;
		justify-content: space-between;
		align-items: center; /* 垂直居中 */
		padding: 30rpx 0;
		min-height: 60rpx;
	}

	.label {
		color: #666;
		font-size: 30rpx;
		width: 240rpx; /* 固定标签宽度 */
		flex-shrink: 0;
	}

	.value {
		color: #333;
		font-size: 30rpx;
		text-align: right;
		flex: 1;
	}
	
	.project-name {
		text-align: right;
		font-weight: 500;
	}

	.placeholder {
		color: #CCC;
	}

	.value-group {
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
	
	.picker-display {
		display: flex;
		align-items: center;
		justify-content: flex-end;
	}

	.picker-display.disabled {
		opacity: 0.7;
	}

	.address-wrapper {
		flex: 1;
		display: flex;
		flex-direction: column;
		align-items: flex-end;
	}

	.address-input {
		width: 100%;
		border: 1rpx solid #E5E5E5;
		border-radius: 8rpx;
		padding: 16rpx;
		font-size: 28rpx;
		color: #333;
		margin-top: 16rpx;
		text-align: right;
	}

	.address-input[disabled] {
		background-color: #F8F8F8;
		color: #999;
	}

	.divider {
		height: 1rpx;
		background-color: #F0F0F0;
		width: 100%;
	}

	.section-title {
		font-size: 34rpx;
		font-weight: bold;
		color: #333;
		margin-bottom: 20rpx;
		padding-left: 10rpx;
	}

	.file-item {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 30rpx 0;
	}

	.file-name {
		color: #333;
		font-size: 30rpx;
	}
</style>
