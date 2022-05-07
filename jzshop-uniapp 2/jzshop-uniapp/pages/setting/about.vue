<template>
	<view class="wrap">
		<view style="text-align: center;">
			<image class="logo" src="/static/img/jzlogo.png" mode="widthFix"></image>
			<view class="version">alpha2020</view>
			<view class="bottom">
				<view>版权信息</view>
				<view>待完善</view>

			</view>
		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		methods: {

		}
	}
</script>

<style lang="scss" scoped>
	.logo {
		margin-top:50rpx;
		width: 300rpx;
		height: 300rpx;
	}

	.version {
		margin-top: 30rpx;
		color: $u-light-color
	}

	.bottom {
		position: absolute;
		bottom: 100rpx;
		width: 100%;
		line-height: 2;
		color: $u-light-color;
	}
</style>
