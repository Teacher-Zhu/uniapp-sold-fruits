<template>
	<view class="wrap">
		<view class="top"></view>

		<view class="content">
			<view class="title">欢迎登录简值</view>

			<!-- adddy by Qingyuan Hu -->
			<u-button type="success" :ripple="true" @click="login">微信一键登录</u-button>

		</view>

		<view class="bottom">
			<view class="loginType">
				<!-- #ifdef APP-PLUS -->
				<view class="wechat item" @click="loginBy('wechat')">
					<view class="icon">
						<u-icon size="70" name="weixin-fill" color="rgb(83,194,64)"></u-icon>
					</view>
					微信
				</view>
				<view class="QQ item" @click="loginBy('QQ')">
					<view class="icon">
						<u-icon size="70" name="qq-fill" color="rgb(17,183,233)"></u-icon>
					</view>
					QQ
				</view>
				<!-- #endif -->
			</view>
			<view class="hint">
				登录代表同意
				<text class="link" @click="goPage('/pages/login/userProtocol')">简值用户协议</text>、
				<text class="link" @click="goPage('/pages/login/privateProtocol')">隐私政策，</text>
				并授权使用您的简值账号信息（如昵称、头像、收获地址）以便您统一管理
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tel: '',
			}
		},
		computed: {
			inputStyle() {
				let style = {};
				if (this.tel && this.tel.length == 11 && this.tel.startsWith('1')) {
					style.color = "#fff";
					style.backgroundColor = this.$u.color['warning'];
				}
				return style;
			}
		},
		onLoad() {},
		methods: {


			// 参考自：https://www.bilibili.com/video/BV1GP4y147es
			login() {
				//let that = this
				uni.getUserProfile({
					desc: '注册',
					success: (res) => {
						console.log("用户信息：" + JSON.stringify(res.userInfo));
						let userInfo = res.userInfo;

						// 	uni.login({
						// 		provider: 'weixin',
						// 		success: (res2) => {
						// 			uni.showLoading({
						// 				title: '登录中'
						// 			})
						// 			// 获得code
						// 			var code = res2.code
						// 			console.log("获取code成功：" + JSON.stringify(res2));
						// 			userInfo.code = code;
						// 			uni.request({
						// 				url: "http://localhost:8081/wechat/getWxOpenId",
						// 				method: 'POST',
						// 				data: userInfo,
						// 				success: (res3) => {
						// 				console.log(res3)
						// 				this.$u.vuex('vuex_token', res3.data.data.token)
						// 				this.$u.vuex('vuex_user', res3.data.data)
						// 				console.log('user', res3.data.data)
						// 				if (res3.data.data.headUrl && res3.data.data.headUrl !== '') {
						// 					console.log('avatar', this.baseApi + '/file/getImgStream?idFile=' + res3.data.data.headUrl);
						// 					// this.$u.vuex('vuex_avatar', this.baseApi + '/file/getImgStream?idFile=' + res3.data.data.headUrl);
						// 					this.$u.vuex('vuex_avatar', 'http://localhost:8081/file/getImgStream?idFile=' + res3.data.data.headUrl);
						// 				};
						// 				this.$u.route({
						// 					type: 'switchTab',
						// 					url: '/pages/user/profile'
						// 				})
						// 				},
						// 				fail:(err)=>{
						// 					console.log(err)
						// 					uni.hideLoading()
						// 					uni.showToast({
						// 						title:'登录失败',
						// 						icon:'none',
						// 					});
						// 				}	
						// 			})

						// 			// added by Qingyuan HU
						// 			// this.$u.post('wechat/getWxOpenId?code=' + code).then(res3 => {
						// 			// 	uni.clearStorageSync()
						// 			// 	this.$u.vuex('vuex_token', res3.token)
						// 			// 	this.$u.vuex('vuex_user', res3.user)
						// 			// 	console.log('user', res3.user)
						// 			// 	if (res3.user.avatar && res3.user.avatar !== '') {
						// 			// 		console.log('avatar', this.baseApi + '/file/getImgStream?idFile=' + res3.user.avatar);
						// 			// 		this.$u.vuex('vuex_avatar', this.baseApi + '/file/getImgStream?idFile=' + res3.user.avatar);
						// 			// 	}
						// 			// 	this.$u.route({
						// 			// 		type: 'switchTab',
						// 			// 		url: '/pages/user/profile'
						// 			// 	})

						// 			// }).catch(res3 => {
						// 			// 	console.log("err", res3)
						// 			// 	this.$u.toast(res3.msg)
						// 			// });



						// 			/* uni.request({
						// 				url: baseApi+'/wechat/getWxOpenId',
						// 				method:'POST',
						// 				header:{
						// 					"content-type":"application/x-www-form-urlencoded",
						// 					"source":"fromApp"
						// 				},
						// 				data:{
						// 					info:JSON.stringify({
						// 						"code":code
						// 						"nickName":userInfo.nickName,
						// 						"gender":userInfo.gender,
						// 						"city":userInfo.city,
						// 						"country":userInfo.country,
						// 						"language":userInfo.language,
						// 						"province":userInfo.language,
						// 						"avatarUrl":userInfo.avatarUrl
						// 					})
						// 				},
						// 				success:(res3)=>{
						// 					console.log('登录成功:'+JSON.stringify(res3));
						// 					uni.hideLoading()

						// 					this.nickName = userInfo.nickName
						// 					this.avatarUrl = userInfo.avatarUrl
						// 				},
						// 				fail:(err)=>{
						// 					console.log(err)
						// 					uni.hideLoading()
						// 					uni.showToast({
						// 						title:'登录失败',
						// 						icon:'none',
						// 					});
						// 				}								
						// 			});	 */


						// 		}
						// 	})
						uni.login({
							provider: 'weixin',
							success: (res2) => {
								uni.showLoading({
									title: '登录中'
								})
								// 获得code
								var code = res2.code
								console.log("获取code成功：" + JSON.stringify(res2));

								userInfo.code = code;

								// added by Qingyuan HU
								this.$u.post('wechat/getWxOpenId', userInfo).then(res3 => {
									console.log(res3)
									uni.clearStorageSync()
									//保存token值
									this.$u.vuex('vuex_token', res3.token)
									this.$u.vuex('vuex_user', res3)
									console.log('user', res3)
									if (res3.headUrl && res3.headUrl !== '') {
										console.log('avatar', this.baseApi +
											'/file/getImgStream?idFile=' + res3.headUrl);
										this.$u.vuex('vuex_avatar', this.baseApi +
											'/file/getImgStream?idFile=' + res3.headUrl);
									}
									this.$u.route({
										type: 'switchTab',
										url: '/pages/user/profile'
									})

								}).catch(res3 => {
									console.log("err", res3)
									this.$u.toast(res3.msg)
								});



							}
						})

					}
				})
			},



			goPage(url) {
				this.$u.route({
					url: url
				})
			},
			passwordLogin() {
				this.$u.route({
					url: '/pages/login/passwordLogin'
				})
			},

		}
	};
</script>

<style lang="scss" scoped>
	.wrap {
		font-size: 28rpx;

		.content {
			width: 600rpx;
			margin: 0 auto;
			padding-top: 80rpx;

			.title {
				text-align: left;
				font-size: 60rpx;
				font-weight: 500;
				margin-bottom: 100rpx;
			}

			input {
				text-align: left;
				margin-bottom: 10rpx;
				padding-bottom: 6rpx;
			}

			.tips {
				color: $u-type-info;
				font-size: 20rpx;
				margin-bottom: 60rpx;
				margin-top: 30rpx;
			}

			.getSmsCode {
				background-color: rgb(253, 243, 208);
				color: $u-tips-color;
				border: none;
				font-size: 30rpx;
				padding: 12rpx 0;

				&::after {
					border: none;
				}
			}

			.alternative {
				color: $u-tips-color;
				display: flex;
				justify-content: space-between;
				margin-top: 30rpx;
			}
		}

		.bottom {
			.loginType {
				display: flex;
				padding: 260rpx 150rpx 150rpx 150rpx;
				justify-content: space-between;

				.item {
					display: flex;
					flex-direction: column;
					align-items: center;
					color: $u-content-color;
					font-size: 28rpx;
				}
			}

			.hint {
				padding: 20rpx 40rpx;
				font-size: 20rpx;
				color: $u-tips-color;

				.link {
					color: $u-type-warning;
				}
			}
		}
	}
</style>
