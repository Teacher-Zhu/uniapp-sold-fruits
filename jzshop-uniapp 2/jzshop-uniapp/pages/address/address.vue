<template>
	<view class="wrap">
		<view class="top">
			<view class="item">
				<view class="left">收货人</view>
				<input type="text" placeholder-class="line" placeholder="请填写收货人姓名" v-model="address.name" />
			</view>
			<view class="item">
				<view class="left">手机号码</view>
				<input type="text" placeholder-class="line" placeholder="请填写收货人手机号" v-model="address.tel" />
			</view>
			<view class="item" @tap="showRegionPicker">
				<view class="left">所在地区</view>
				<input disabled type="text" placeholder-class="line" placeholder="省市区县、乡镇等" v-model="addressShowValue" />
			</view> 
			
			<!-- added by Qingyuan HU -->
			<view class="item" @tap="showPickPicker">
				<view class="left">自提点</view>
				<input disabled type="text" placeholder-class="line" placeholder="就近的社区/园区自提点" v-model="pickShowValue" />
			</view>
			
		</view>
		<view class="bottom">			 
			<view class="default">
				<view class="left">
					<view class="set">设置默认地址</view>
					<view class="tips">提醒：每次下单会默认推荐该地址</view>
				</view>
				<view class="right"><u-switch v-model="address.isDefault" @change = "changeDefault"></u-switch></view>
			</view>
			
		</view>
		<u-button type="error" shape="circle" @click="save">保存</u-button>
		<br>
		<u-button type="default" shape="circle" v-if="id!=''" @click="del">删除</u-button>
		<u-picker mode="region" ref="uPicker" v-model="show" :area-code="defaultRegion" @confirm="confirmArea" />
		<u-select :default-value="defaultPick" mode="mutil-column-auto" v-model="pickShow" :list="pickList" @confirm="confirmPick"></u-select>
	</view>
</template>

<script>
export default {
	data() {
		return {
			choose:true,
			show: false,
			id:'',
			address:{isDefault:false},
			defaultRegion:['','',''],
			defaultPick:[,,],
			pickShow: false,					
			pickList: [
			{
				value: 32021101,
				label: '江南大学',
				children: [
				{
					value: 3202110101,
					label: '北区',
					children: [
					{
						value: 320211010101,
						label: '李园'
					},
					{
						value: 3202110102,
						label: '榴园'
					}]
				},
				{
					value: 3202110102,
					label: '南区',
					children: [
					{
						value: 320211010201,
						label: '涓苑'
					},
					{
						value: 320211010202,
						label: '澈苑'
					}]
				}],
			},
			{
				value: 32021102,
				label: '职院',
				children: [{
					value: 3202110201,
					label: 'A区',
					children: [{
						value: 320211020101,
						label: 'A1自提点'
					}]
				}]
			}
			]
				
		};
	},
	computed:{
		addressShowValue(){			
			if(this.address.province){
			return this.address.province+' '+this.address.city+' '+this.address.district;
			}
		},
		pickShowValue(){
			if(this.address.street){
			return this.address.street+' '+this.address.community+' '+this.address.pickpoint;
			}
		}
	},
	onLoad(option){
		this.id = option.id 
		if(option.choose){
			this.choose = option.choose
		}
		if(this.id){
			this.init()
		}else{
			this.id = ''
			//显示默认的地区
			this.defaultRegion = ["32", "3202", "320211"]
			this.defaultPick = [0,0,0]
		}
	},
	methods: {
		init(){
			this.$u.get('user/address/'+this.id).then( res => {
				const code1 = res.areaCode.substr(0,2)
				const code2 = res.areaCode.substr(0,4)
				const code3 = res.areaCode.substr(0,6)
				const code4 = new Number(res.areaCode.substr(0,8)).parseInt()
				const code5 = new Number(res.areaCode.substr(0,10)).parseInt()
				const code6 = new Number(res.areaCode).parseInt()
				this.defaultRegion = [code1,code2,code3]
				this.defaultPick = [code4,code5,code6]
				this.address = res
			});
		},
		setDefault() {},
		showRegionPicker() {
			this.show = true
		},
		showPickPicker(){
			this.pickShow = true
		},
		confirmArea(e){		
			this.address.areaCode = e.area.value
			//设置选择器默认值
			this.defaultRegion = [e.province.value,e.city.value,e.area.value]
			//设置输入框值
			this.address =   Object.assign({},this.address)
			this.address.province = e.province.label
			this.address.city = e.city.label
			this.address.district = e.area.label
		},
		confirmPick(e){
			this.address.pickCode = e[2].value
			//设置选择器默认值
			this.defaultPick = [e[0].value,e[1].value,e[2].value]
			//设置输入框值
			this.address =   Object.assign({},this.address)
			this.address.street = e[0].label
			this.address.community = e[1].label
			this.address.pickpoint = e[2].label
		},
		changeDefault(e){
			if(this.address.id){
				this.$u.post('user/address/'+this.address.id+'/'+e).then( res => {
					this.$u.toast(e?'设置默认地址':'取消默认地址')
				})
			}
		},
		save(){
			this.$u.post('user/address/save',this.address).then( res => {
				if(this.choose){
					let idCarts = uni.getStorageSync('idCarts')
					if (idCarts && idCarts !== '') {
						uni.setStorageSync('chooseAddrId', res.id);
						 this.$u.route({
						 	url:'/pages/checkout/checkout',
						 	params:{
						 		ids:idCarts
						 	}
						 })
					}else{
						this.$u.route('/pages/address/list')
					}
					
				}else{
					this.$u.route('/pages/address/list')
				}
			})
		},
		del(){
			this.$u.delete('user/address/'+this.address.id).then( res => {
				this.$u.route('/pages/address/list')
			})
		}
	}
};
</script>

<style lang="scss" scoped>
 
.wrap { 
	padding:0rpx 30rpx;
	.top { 
		padding: 22rpx;
		.item {
			display: flex;
			font-size: 32rpx;
			line-height: 100rpx;
			align-items: center;
			border-bottom: solid 2rpx $u-border-color;
			.left {
				width: 180rpx;
			}
			input {
				text-align: left;
			}
		}
		
		.address {
			padding: 20rpx 0;
			textarea {
				// width: 100%;
				height: 150rpx;
				background-color: #f7f7f7;
				line-height: 60rpx;
				margin: 40rpx auto;
				padding: 20rpx;
			}
		}
		.site-clipboard {
			padding-right: 40rpx;
			textarea {
				// width: 100%;
				height: 150rpx;
				background-color: #f7f7f7;
				line-height: 60rpx;
				margin: 40rpx auto;
				padding: 20rpx;
			}
			.clipboard {
				display: flex;
				justify-content: center;
				align-items: center;
				font-size: 26rpx;
				color: $u-tips-color;
				height: 80rpx;
				.icon {
					margin-top: 6rpx;
					margin-left: 10rpx;
				}
			}
		}
	}
	.bottom {
		margin-top: 20rpx;
		padding: 40rpx;
		padding-right: 0;
		background-color: #ffffff;
		font-size: 28rpx;
		.tag {
			display: flex;
			.left {
				width: 160rpx;
			}
			.right {
				display: flex;
				flex-wrap: wrap;
				.tags {
					width: 140rpx;
					padding: 16rpx 8rpx;
					border: solid 2rpx $u-border-color;
					text-align: center;
					border-radius: 50rpx;
					margin: 0 10rpx 20rpx;
					display: flex;
					font-size: 28rpx;
					align-items: center;
					justify-content: center;
					color: $u-content-color;
					line-height: 1;
				}
				.plus {
					//padding: 10rpx 0;
				}
			}
		}
		.default {
			margin-top: 50rpx;
			display: flex;
			justify-content: space-between;
			border-bottom: solid 2rpx $u-border-color;
			line-height: 64rpx;
			.tips {
				font-size: 24rpx;
			}
			.right {
			}
		}
	}
}
</style>
