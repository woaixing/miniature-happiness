<template>
	<view>
		<image class="integral-role-header" :src="statics.integralRoleHeader"></image>
		<view class="integral-role-main pd16_15">
			<view class="box pd16_15">
				<view class="flex alcenter space pb16 bd-bottom mb16">
					<view class="flex alcenter">
						<image :src="statics.integralRoleImg[0]" style="width: 100rpx;height: 100rpx;"></image>
						<view class="ml15">
							<view class="ft14 ftw600">注册完善资料</view>
							<view class="flex alcenter mt12">
								<image :src="statics.integralImg01" style="width: 32rpx; height: 32rpx;"></image>
								<view class="ft12 cl-notice ml5">奖励10积分</view>
							</view>
						</view>
					</view>
					<view class="btn-small" :style="getBtnDisStyle" style="width: 148rpx;">已赚到</view>
				</view>
				<view class="flex alcenter space pb16 bd-bottom mb16">
					<view class="flex alcenter">
						<image :src="statics.integralRoleImg[1]" style="width: 100rpx;height: 100rpx;"></image>
						<view class="ml15">
							<view class="ft14 ftw600">邀请好友注册</view>
							<view class="flex alcenter mt12">
								<image :src="statics.integralImg01" style="width: 32rpx; height: 32rpx;"></image>
								<view class="ft12 cl-notice ml5">奖励10积分</view>
							</view>
						</view>
					</view>
					<button open-type="share" class="btn-small" :style="getBtnRoleStyle" style="width: 148rpx;">赚积分</button>
				</view>
				<view class="flex alcenter space pb16 bd-bottom mb16">
					<view class="flex alcenter">
						<image :src="statics.integralRoleImg[2]" style="width: 100rpx;height: 100rpx;"></image>
						<view class="ml15">
							<view class="ft14 ftw600">充值余额</view>
							<view class="flex alcenter mt12">
								<image :src="statics.integralImg01" style="width: 32rpx; height: 32rpx;"></image>
								<view class="ft12 cl-notice ml5">奖励对应充值积分</view>
							</view>
						</view>
					</view>
					<navigator url="/pages/client/vipcard/recharge">
					<view class="btn-small" :style="getBtnRoleStyle" style="width: 148rpx;">赚积分</view>
					</navigator>
				</view>
				<view class="flex alcenter space pb16 bd-bottom mb16">
					<view class="flex alcenter">
						<image :src="statics.integralRoleImg[3]" style="width: 100rpx;height: 100rpx;"></image>
						<view class="ml15">
							<view class="ft14 ftw600">购买次卡</view>
							<view class="flex alcenter mt12">
								<image :src="statics.integralImg01" style="width: 32rpx; height: 32rpx;"></image>
								<view class="ft12 cl-notice ml5">奖励对应次卡积分</view>
							</view>
						</view>
					</view>
					<navigator url="/pages/client/card/index">
					<view class="btn-small" :style="getBtnRoleStyle" style="width: 148rpx;">赚积分</view>
					</navigator>
				</view>
				<view class="flex alcenter space pb16 bd-bottom mb16">
					<view class="flex alcenter">
						<image :src="statics.integralRoleImg[4]" style="width: 100rpx;height: 100rpx;"></image>
						<view class="ml15">
							<view class="ft14 ftw600">购买特惠套餐</view>
							<view class="flex alcenter mt12">
								<image :src="statics.integralImg01" style="width: 32rpx; height: 32rpx;"></image>
								<view class="ft12 cl-notice ml5">奖励对应消费积分</view>
							</view>
						</view>
					</view>
					<navigator url="/pages/client/tuan/index">
					<view class="btn-small" :style="getBtnRoleStyle" style="width: 148rpx;">赚积分</view>
					</navigator>
				</view>
				<view class="flex alcenter space pb16 ">
					<view class="flex alcenter">
						<image :src="statics.integralRoleImg[5]" style="width: 100rpx;height: 100rpx;"></image>
						<view class="ml15">
							<view class="ft14 ftw600">订单评价</view>
							<view class="flex alcenter mt12">
								<image :src="statics.integralImg01" style="width: 32rpx; height: 32rpx;"></image>
								<view class="ft12 cl-notice ml5">奖励10积分</view>
							</view>
						</view>
					</view>
					<navigator url="/pages/client/order/index">
					<view class="btn-small" :style="getBtnRoleStyle" style="width: 148rpx;">赚积分</view>
					</navigator>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return {
				
			}
		},
		computed:{
			getBtnRoleStyle(){
				let style = 'background: linear-gradient(135deg, #FFBB00 0%, #FF4D00 100%);color:#ffffff;';
				return  style;
			}
		},
		onLoad(){
			
		},
		onShareAppMessage(e){
			
		},
		onShareTimeline(e){
			
		},
		methods:{
			
		}
	}
</script>

<style>
	.integral-role-header{
		position: relative;
		width: 100%;
		height: 400rpx;
	}
	.integral-role-main{
		position: relative;
		width: 100%;
		margin-top: -192rpx;
	}
</style>