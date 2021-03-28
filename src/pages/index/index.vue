<template>
	<view class="content">
		<!-- #ifdef MP-WEIXIN -->
		<button open-type="getUserInfo" @getuserinfo="getUserInfo">
			授权用户信息
		</button>
		<!-- #endif -->
		<!-- #ifdef MP-WEIXIN -->
		<button open-type="getPhoneNumber" @getphonenumber="getPhoneNumber" withCredentials="true">
			授权手机号
		</button>
		<!-- #endif -->
		<button @click="subscribeMessage">订阅消息</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		onLoad() {

		},
		methods: {
			// 获取用户信息
			getUserInfo() {
				// 授权登录
				uni.login({
					provider: 'weixin',
					success: function(loginRes) {
						console.log("授权登录信息：", loginRes);
						// 获取用户信息
						uni.getUserInfo({
							provider: 'weixin',
							success: function(infoRes) {
								console.log("授权用户详细信息：", infoRes);
								console.log('用户昵称为：' + infoRes.userInfo.nickName);
							}
						});
					}
				});
			},
			// 获取用户手机号
			getPhoneNumber(phoneRes) {
				// 授权登录
				uni.login({
					provider: 'weixin',
					success: function(loginRes) {
						console.log("授权登录信息：", loginRes);
						if (phoneRes.detail.errMsg == "getPhoneNumber:ok") {
							console.log("用户手机号信息：", phoneRes);
						}
					}
				});
			},
			// 订阅消息
			subscribeMessage() {
				uni.getSetting({
						// 是否获取用户订阅消息的订阅状态，默认false不返回
						withSubscriptions: true,
						success(res) {
							console.log("授权设置", res)
							if (!res.subscriptionsSetting['mainSwitch']) {
								// 打开设置页
								uni.openSetting({
									success(res) {
										console.log("授权设置", res)
									}
								});
							} else {
								// 用户没有点击”总是保持以上，不再询问“则每次都会调起订阅消息
								uni.requestSubscribeMessage({
										tmplIds: ["模板ID","模板ID"
											],
											success(res) {
												console.log("订阅模板", res)
											}
										})
								}
							}
						})
					// 结束
				},
			}
		}
</script>

<style lang="scss" scoped>

</style>
