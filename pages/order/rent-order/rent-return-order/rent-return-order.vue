<template>
	<view class="edit-address">
		<form @submit="formSubmit" @reset="formReset">
			<view class="edit-address-item">
				<view>取件人</view>
				<input type="text" placeholder="请输入取件人姓名" name="consignee">
			</view>
			<view class="edit-address-item">
				<view>手机号</view>
				<input type="text" placeholder="请输入收货人手机号码" name="consigneePhone">
			</view>
			<view class="edit-address-item">
				<view>归还时间</view>
				<picker mode="date" @change="pickerChange">
					<input type="text" placeholder="请选择时间" v-model="returnTime" name="returnTime">
				</picker>
			</view>
			<view class="edit-address-item">
				<view>取件地址</view>
				<input type="text" placeholder="请输入详细地址" name="detailedAddress">
			</view>
			<button type="primary" form-type="submit">确定</button>
		</form>
	</view>
</template>

<script>
	export default {
		onLoad(options) {
			console.log(options)
		},
		data() {
			return {
				returnTime:""
			}
		},
		methods: {
			pickerChange(event){
				console.log(event)
				this.returnTime = event.detail.value
			},
			formSubmit(event){
				console.log(event.detail)
				// uni.request();
			}
		}
	}
</script>
<style>
	page {
		background-color:#f9f9f9;
	}
</style>
<style lang="scss" scoped>
.edit-address{
	.edit-address-item{
		margin-left: 40upx;
		padding: 40upx 40upx 40upx 0;
		display: flex;
		align-items: center;
		justify-content: space-between;
		height :26upx;
		font-size:26upx;
		border-bottom:1upx #ececec solid;
		input{
			width: 500upx;
			font-size:26upx;
		}
		.select-area{
			width: 498upx;
			height: 100%;
			font-size:26upx;
		}
	}
	checkbox-group{
		checkbox{
			padding: 40upx;
		}
		.default-address-text{
			font-size: 30upx;
		}
	}
}

</style>
