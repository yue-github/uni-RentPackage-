<template>
	<view class="add-address">
		<form @submit="formSubmit" @reset="formReset">
			<view class="add-address-item">
				<view>收货人</view>
				<input type="text" placeholder="请输入收货人姓名" name="consignee">
			</view>
			<view class="add-address-item">
				<view>手机号</view>
				<input type="text" placeholder="请输入收货人手机号码" name="consigneePhone">
			</view>
			<view class="add-address-item">
				<view>省市区</view>
				
				<!--  #ifdef  MP-WEIXIN -->
				<picker mode="region" @change="pickerChange">
					<input type="text" placeholder="请输入地区" v-model="provinces" name="provinces">
				</picker>
				<!--  #endif -->
				
				<!--  #ifdef  APP-PLUS || H5 -->
				<picker mode="multiSelector" :range="multiArray"  @columnchange="bindMultiPickerColumnChange" :value="multiIndex" @change="pickerChange" >
					<view class="select-area">{{provinces}}</view>
				</picker>
				<!--  #endif -->
			</view>
			<view class="add-address-item">
				<view>详细地址</view>
				<input type="text" placeholder="请输入详细地址" name="detailedAddress">
			</view>
			<checkbox-group name="defaultAddress">
				<checkbox value="true" id="defaultAddress"></checkbox>
				<label for="defaultAddress">
					<text class="default-address-text">默认地址</text>
				</label>
			</checkbox-group>
			<button type="primary" form-type="submit">确定</button>
		</form>
	</view>
</template>

<script>
	import Provinces from '../Provinces.json'
	export default {
		data() {
			return {
				provinces:"请选择地区",
				// #ifdef  APP-PLUS || H5
				multiArray: [
					[],
					[],
					[]
				],
				multiIndex: [0, 0, 0],
				// #endif
			}
		},
		created() {
			//将multiArray初始化
			// #ifdef  APP-PLUS || H5
			let province = []
			let city = []
			let counties = []
			for (let i = 0; i < Provinces.length; i++) {
				province.push(Provinces[i].name)
			}
			for (let i = 0; i < Provinces[0].city.length; i++) {
				city.push(Provinces[0].city[i].name)
			}
			for (let i = 0; i < Provinces[0].city[0].area.length; i++) {
				counties.push(Provinces[0].city[0].area[i].name)
			}
			this.multiArray[0]=province
			this.multiArray[1]=city
			this.multiArray[2]=counties
			// #endif	
		},
		methods: {
			pickerChange(event){
				// #ifdef  APP-PLUS || H5
					let pickerDate = ['','','']
					for(let i = 0 ; i < pickerDate.length ; i++){
						pickerDate[i]=this.multiArray[i][this.multiIndex[i]]
					}
					event.detail.value = pickerDate
				// #endif
				this.provinces = event.detail.value.join(" ")
			},
			formSubmit(event){
				console.log(event.detail)
				// uni.request();
			},
			
			// #ifdef  APP-PLUS || H5
			bindMultiPickerColumnChange(e){
				let city = []
				let counties = []
				
				//保存上一次两个Index的值用于判断是否发生变化
				let provinceIndex = this.multiIndex[0]
				let cityIndex = this.multiIndex[1]
				
				//用multiIndex保持当前状态,方便后面对状态的改变
				this.multiIndex[e.detail.column] = e.detail.value
				
				//判断省份是否变化,如果改变将市,县区改变成首项
				if(provinceIndex!=this.multiIndex[0]){
					this.multiIndex=[this.multiIndex[0],0,0]
				}
				
				//判断市是否变化,如果改变县区改变成首项
				if(cityIndex!=this.multiIndex[1]){
					this.multiIndex=[this.multiIndex[0],this.multiIndex[1],0]
				}
				
				switch(this.multiIndex[0]){
					//监听第一列改变
					case this.multiIndex[0]:
						for (var i = 0; i < Provinces[this.multiIndex[0]].city.length; i++) {
							city.push(Provinces[this.multiIndex[0]].city[i].name)
						}
						this.multiArray[1]=city
						switch(this.multiIndex[1]){
							//监听第二列的改变
							case this.multiIndex[1]:
								for (var i = 0; i < Provinces[this.multiIndex[0]].city[this.multiIndex[1]].area.length; i++) {
									counties.push(Provinces[this.multiIndex[0]].city[this.multiIndex[1]].area[i].name)
								}
								this.multiArray[2]=counties
							break;
						}
					break;
				}
			}
			// #endif
		}
	}
</script>
<style>
	page {
		background-color:#f9f9f9;
	}
</style>
<style lang="scss" scoped>
.add-address{
	.add-address-item{
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
