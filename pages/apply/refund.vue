<template>
	<view class="container">
		<view class="search-header">
			<view class="search-bar">
				<icon type="search" size="14" color="#999"></icon>
				<input type="text" placeholder="请输入项目名称" placeholder-class="placeholder" />
				<icon type="clear" size="14" color="#ccc"></icon>
			</view>
			<text class="cancel-btn">取消</text>
		</view>

		<!-- 顶部 Tab 模拟（如果是从 Tab 页跳过来的，可能不需要这个，但看截图顶部有 Tab 栏，不过为了表单页面的独立性，这里仅展示表单内容，保持页面简洁）-->
		<!-- 截图里顶部确实有 Tab，说明可能是在 apply.vue 里的某个状态？但通常表单页是独立的。为了还原截图的表单结构，我主要实现表单部分 -->

		<scroll-view scroll-y="true" class="form-container">
			<!-- 保函信息 -->
			<view class="section-title">保函信息</view>
			<view class="card">
				<view class="form-item">
					<text class="label">合同开始日期</text>
					<picker mode="date" :value="formData.startDate" @change="bindDateChange($event, 'startDate')">
						<view class="picker-display">
							<text :class="['value', formData.startDate ? '' : 'placeholder']">{{ formData.startDate || '年/月/日' }}</text>
							<text class="icon-calendar">&#128197;</text>
						</view>
					</picker>
				</view>
				<view class="divider"></view>

				<view class="form-item">
					<text class="label">合同到期日期</text>
					<picker mode="date" :value="formData.endDate" @change="bindDateChange($event, 'endDate')">
						<view class="picker-display">
							<text :class="['value', formData.endDate ? '' : 'placeholder']">{{ formData.endDate || '年/月/日' }}</text>
							<text class="icon-calendar">&#128197;</text>
						</view>
					</picker>
				</view>
				<view class="divider"></view>

				<view class="form-item">
					<text class="label">是否需要开发票</text>
					<picker mode="selector" :range="invoiceOptions" :value="invoiceIndex" @change="bindInvoiceChange">
						<view class="picker-display">
							<text class="value">{{ invoiceOptions[invoiceIndex] }}</text>
							<text class="arrow-down">∨</text>
						</view>
					</picker>
				</view>
				<view class="divider"></view>

				<view class="form-item">
					<text class="label">保证金金额 (元)</text>
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
					<text class="value-static">深圳致和易保科技有限公司</text>
				</view>
				<view class="divider"></view>

				<view class="form-item">
					<text class="label">被保证人联系人</text>
					<text class="value-static">林大大</text>
				</view>
				<view class="divider"></view>

				<view class="form-item">
					<text class="label">被保证人联系人电话</text>
					<text class="value-static">18954655511</text>
				</view>
				<view class="divider"></view>

				<view class="form-item">
					<text class="label">被保证人法人名称</text>
					<text class="value-static">胡伟雄</text>
				</view>
				<view class="divider"></view>
				
				<view class="form-item">
					<text class="label">统一社会信用代码</text>
					<text class="value-static placeholder">请输入</text>
				</view>
			</view>

			<!-- 申请资料 -->
			<view class="section-title">请上传申请资料</view>
			<view class="card">
				<view class="file-list">
					<view class="file-item">
						<text class="file-label">营业执照</text>
						<text class="file-status">营业执照.png</text>
					</view>
					<view class="divider"></view>

					<view class="file-item">
						<text class="file-label">资质证明</text>
						<text class="file-status">资质证明.png</text>
					</view>
					<view class="divider"></view>

					<view class="file-item">
						<text class="file-label">法人身份证</text>
						<text class="file-status">法人身份证国徽面.png、人像面.png</text>
					</view>
					<view class="divider"></view>

					<view class="file-item" @click="uploadFile('近两年审计报告')">
						<text class="file-label">近两年审计报告</text>
						<text class="upload-action">去上传 ></text>
					</view>
					<view class="divider"></view>

					<view class="file-item">
						<text class="file-label">中标通知书</text>
						<text class="file-status">中标通知书.pdf</text>
					</view>
					<view class="divider"></view>
					
					<view class="file-item" @click="uploadFile('保险保函申请书')">
						<text class="file-label">保险保函申请书</text>
						<text class="upload-action">去上传 ></text>
					</view>
					<view class="divider"></view>
					
					<view class="file-item" @click="uploadFile('施工总承包单位承诺书')">
						<text class="file-label">施工总承包单位承诺书</text>
						<text class="upload-action">去上传 ></text>
					</view>
				</view>
			</view>
		</scroll-view>

		<!-- 底部按钮 -->
		<view class="footer-buttons">
			<button class="btn-cancel" @click="goBack">取消</button>
			<button class="btn-submit" @click="submitForm">提交</button>
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
					amount: '',
				},
				invoiceOptions: ['否', '是'],
				invoiceIndex: 0
			}
		},
		onLoad() {
			uni.setNavigationBarTitle({
				title: '申请保证金'
			});
		},
		methods: {
			bindDateChange(e, field) {
				this.formData[field] = e.detail.value;
			},
			bindInvoiceChange(e) {
				this.invoiceIndex = e.detail.value;
			},
			uploadFile(name) {
				uni.showToast({
					title: '点击了上传: ' + name,
					icon: 'none'
				});
			},
			goBack() {
				uni.navigateBack();
			},
			submitForm() {
				uni.showToast({
					title: '提交成功',
					icon: 'success'
				});
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
		padding-bottom: 120rpx;
	}

	/* 搜索栏样式复用 */
	.search-header {
		background-color: #FFFFFF;
		padding: 20rpx 30rpx;
		display: flex;
		align-items: center;
		margin-bottom: 20rpx;
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

	.cancel-btn {
		font-size: 30rpx;
		color: #666;
		margin-left: 20rpx;
	}

	.form-container {
		padding: 0 20rpx;
	}

	.section-title {
		font-size: 30rpx;
		color: #333;
		font-weight: bold;
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
		min-height: 60rpx;
	}

	.label {
		color: #333;
		font-size: 30rpx;
		flex-shrink: 0;
		width: 280rpx;
	}

	.value, .input-value, .value-static {
		color: #333;
		font-size: 30rpx;
		text-align: right;
		flex: 1;
	}
	
	.value-static.placeholder {
		color: #CCC;
	}

	.file-value {
		color: #333;
		font-size: 30rpx;
		text-align: right;
		flex: 1;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
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

	.icon-calendar, .arrow-down {
		margin-left: 10rpx;
		font-size: 32rpx;
		color: #999;
	}

	.divider {
		height: 1rpx;
		background-color: #F0F0F0;
		width: 100%;
	}

	/* 文件列表 */
	.file-item {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 30rpx 0;
	}

	.file-label {
		color: #333;
		font-size: 30rpx;
		width: 240rpx;
		flex-shrink: 0;
	}

	.file-status {
		color: #666;
		font-size: 28rpx;
		text-align: right;
		flex: 1;
	}

	.upload-action {
		color: #999;
		font-size: 28rpx;
		text-align: right;
		flex: 1;
	}

	/* 底部按钮 */
	.footer-buttons {
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;
		display: flex;
		padding: 20rpx 30rpx;
		padding-bottom: 50rpx; /* 适配底部安全区 */
		background-color: #FFFFFF;
		box-shadow: 0 -2rpx 10rpx rgba(0,0,0,0.05);
		z-index: 100;
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
		background-color: #A9906A; /* 金色 */
		color: #FFFFFF;
		border: none;
	}
	
	.btn-cancel::after, .btn-submit::after {
		border: none;
	}
</style>
