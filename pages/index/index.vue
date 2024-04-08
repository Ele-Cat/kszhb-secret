<template>
	<view class="content">
		<view class="text-area" @click="handleClick">
			<text class="title">{{secretKey}}</text>
			<text class="tip">点击快捷复制</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				timer: null,
				secretKey: "",
				checkPeriod: 3 * 1000,
			}
		},
		onShow() {
			this.getSecretKey()
			this.timer && clearInterval(this.timer)
			this.timer = setInterval(() => {
				this.getSecretKey()
			}, this.checkPeriod)
		},
		methods: {
			getSecretKey() {
				let currentDate = new Date();
				if (currentDate.getHours() >= 20) {
					currentDate.setDate(currentDate.getDate() + 1);
				}
				currentDate.setHours(8, 0, 0, 0);
				let secretKey = btoa(currentDate.getTime() / 1000);
				console.log('secretKey', secretKey)
				this.secretKey = secretKey
			},
			handleClick() {
				uni.setClipboardData({
					data:	this.secretKey
				})
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.text-area {
		margin-top: 200rpx;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	.title {
		font-size: 36rpx;
		color: #333;
		margin: 24rpx 0;
	}
	
	.tip {
		font-size: 24rpx;
		color: #999;
	}
</style>
