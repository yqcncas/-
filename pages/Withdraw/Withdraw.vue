<template>
	<view class="Withdraw">
		<view class="Withdraw-header">提现方式</view>
		<view class="account">
			<view class="account-top">
				<view class="account-top-left">支付宝账号*</view>
				<input type="text" v-model="userAccount" placeholder="请输入支付宝账号" placeholder-style="font-family: PingFangSC-Regular;font-size: 14px;color: #A3A3A3;letter-spacing: -0.34px;" />
			</view>
			<view class="account-top">
				<view class="account-top-left">真实姓名*</view>
				<input type="text" v-model="userName" placeholder="请输入真实姓名" placeholder-style="font-family: PingFangSC-Regular;font-size: 14px;color: #A3A3A3;letter-spacing: -0.34px;" />
			</view>
			<view class="line-3"></view>
			<view class="can-Withdraw">
				<view class="can-Withdraw-top">
					<view class="can-Withdraw-top-left">提现金额*</view>
					<input type="number"  v-model="withdrawMoney"  placeholder="单行输入" placeholder-style="font-family: PingFangSC-Regular;font-size: 14px;color: #A3A3A3;letter-spacing: -0.34px;" />
				</view>
				<view class="can-Withdraw-bottom">
					<view class="can-Withdraw-bottom-left">可提现金额</view>
					<input disabled="true" type="number"  :placeholder="canMoney" placeholder-style="font-family: PingFangSC-Regular;font-size: 14px;color: #A3A3A3;letter-spacing: -0.34px;" />
				</view>
			</view>
		</view>
		<view class="submit-Withdraw" @click="Withdraw">确认提现</view>
	</view>
</template>

<script>
	export default {
		onLoad(options) {
			this.canMoney = options.money
		},
		data () {
			return {
				userAccount: '',
				userName: '',
				withdrawMoney: '',
				canMoney: ''
			}
		},
		methods: {
			Withdraw () {
				if (this.userAccount.trim() == '') {
					return uni.showToast({
						icon: 'none',
						title: '请填写支付宝账号'
					})
				}
				if (this.userName.trim() == '') {
					return uni.showToast({
						icon: 'none',
						title: '请填写姓名'
					})
				}
				if (this.withdrawMoney.trim() == '') {
					return uni.showToast({
						icon: 'none',
						title: '请填写提现金额'
					})
				}
				if (this.withdrawMoney <= 0) {
					return uni.showToast({
						icon: 'none',
						title: '请填写正确金额'
					})
				}
				if (Number(this.withdrawMoney) > Number(this.canMoney)) {
					return uni.showToast({
						icon: 'none',
						title: '超出可提取上限'
					})
				}
				
				if (Math.floor(this.withdrawMoney) < 10) {
					return uni.showToast({
						icon: 'none',
						title: '最低提现额度为10元'
					})
				}
				
				if (!this.$u.test.chinese(this.userName)) {
					return uni.showToast({
						icon: 'none',
						title: '真实姓名必须为汉字'
					})
				}
				
				uni.showModal({
				    title: '提示',
				    content: '是否确认提现',
				    success:  async(res) => {
				        if (res.confirm) {
				            let msg = await this.$fetch(this.$api.add_draw_apply, {account: this.userAccount, amount: this.withdrawMoney, userName: this.userName}, 'POST', 'FORM')
							uni.showToast({
								icon: 'none',
								title: msg.msg
							})
							setTimeout(() => {
								uni.navigateBack({
									delta: 1
								})
							}, 500)
						
				        } else if (res.cancel) {
				            console.log('用户点击取消');
				        }
				    }
				});
			}
		}
	}
</script>

<style lang="less">
	.Withdraw{
		.Withdraw-header{
			padding-top: 32rpx;
			padding-left: 34rpx;
			box-sizing: border-box;
			font-family: PingFangSC-Medium;
			font-size: 16px;
			color: #141414;
			letter-spacing: -0.39px;
			font-weight: bold;
			padding-bottom: 32rpx;
			box-sizing: border-box;
		}
		.account{
			.account-top{
				display: flex;
				align-items: center;
				padding-left: 36rpx;
				padding-right: 36rpx;
				box-sizing: border-box;
				&:nth-child(2){
					padding-top: 56rpx;
					padding-bottom: 22rpx;
					box-sizing: border-box;
				}
				.account-top-left{
					width: 202rpx;
					font-family: PingFangSC-Regular;
					font-size: 14px;
					color: #141414;
					letter-spacing: -0.34px;
				}
				input{
					flex: 1;
				}
			}
		}
		.can-Withdraw{
			padding-top: 32rpx;
			padding-left: 36rpx;
			padding-right: 36rpx;
			box-sizing: border-box;
			.can-Withdraw-top{
				display: flex;
				align-items: center;
				padding-bottom: 34rpx;
				box-sizing: border-box;
				.can-Withdraw-top-left{
					width: 202rpx;
				}
				input {
					flex: 1;
				}
			}
			.can-Withdraw-bottom{
				display: flex;
				align-items: center;
				width: 660rpx;
				height: 88rpx;
				background: #F1F1F1;
				border-radius: 4px;
				overflow: hidden;
				padding: 24rpx;
				box-sizing: border-box;
				.can-Withdraw-bottom-left{
					width: 180rpx;
					font-family: PingFangSC-Regular;
					font-size: 14px;
					color: #909090;
					letter-spacing: -0.34px;
				}
				input{
					flex: 1;
				}
			}
		}
		.submit-Withdraw{
			background-image: linear-gradient(136deg, #FF8D3F 0%, #E86D29 100%);
			border-radius: 4px;
			border-radius: 4px;
			width: 604rpx;
			height: 102rpx;
			line-height: 102rpx;
			text-align: center;
			font-family: PingFangSC-Medium;
			font-size: 21px;
			color: #FFFFFF;
			letter-spacing: 1.19px;
			position: fixed;
			left: 50%;
			transform: translateX(-50%);
			bottom: 30rpx;
		}
	}
</style>
