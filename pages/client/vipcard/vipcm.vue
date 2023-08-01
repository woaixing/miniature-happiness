<template>
	<view class="pd16_15">
		
		<block v-for="(item,index) in moneyList" :key="index">
		<view v-if="item.group_points_day+item.group_points_week+item.group_points_month+item.group_points_year>0"  :class="index==1?'pb15':'pb15 pt10'" style="border-bottom: 1px solid #ddd;">
			<view class="pd5_15 ft18 cl-main ftw600">{{item.group_name}}</view>>
			<view class="mt5 flex space" style="display:flex; flex-wrap:wrap;">
				<view class="pd10 mt16" v-if="item.group_points_day>0" @click="paySuccess(item.group_id,item.group_points_day,'day')" style=" border-radius: 8px; width: 49%; background: #f2854c;">
					<view class="ft14 cl-main ftw600" style="color: rgba(255,255,255,0.9); text-align: center;">{{item.group_name}}-包天</view>
					<view class="ft16 cl-main ftw600" style="color: rgba(255,255,255,0.9); text-align: center;">【{{item.group_points_day}}积分】</view>
				</view>
				<view class="pd10 mt16" v-if="item.group_points_week>0" @click="paySuccess(item.group_id,item.group_points_week,'week')" style=" border-radius: 8px; width: 49%; background: #36b1c0;">
					<view class="ft14 cl-main ftw600" style="color: rgba(255,255,255,0.9); text-align: center;">{{item.group_name}}-包周</view>
					<view class="ft16 cl-main ftw600" style="color: rgba(255,255,255,0.9); text-align: center;">【{{item.group_points_week}}积分】</view>
				</view>
				<view class="pd10 mt16" v-if="item.group_points_month>0" @click="paySuccess(item.group_id,item.group_points_month,'month')" style=" border-radius: 8px; width: 49%; background: #f2395b;">
					<view class="ft14 cl-main ftw600" style="color: rgba(255,255,255,0.9); text-align: center;">{{item.group_name}}-包月</view>
					<view class="ft16 cl-main ftw600" style="color: rgba(255,255,255,0.9); text-align: center;">【{{item.group_points_month}}积分】</view>
				</view>
				<view class="pd10 mt16" v-if="item.group_points_year>0" @click="paySuccess(item.group_id,item.group_points_year,'year')" style=" border-radius: 8px; width: 49%; background: #853f8a;">
					<view class="ft14 cl-main ftw600" style="color: rgba(255,255,255,0.9); text-align: center;">{{item.group_name}}-包年</view>
					<view class="ft16 cl-main ftw600" style="color: rgba(255,255,255,0.9); text-align: center;">【{{item.group_points_year}}积分】</view>
				</view>
			</view>
		</view>
		</block>	
		<!-- <view class="form-footer-h">
			<view class="form-footer form-footer-h">
				<view class="form-footer-main pd10_15">
					<button @click="paySuccess" class="btn-big" :style="getBtnStyle">确定</button>
				</view>
			</view>
		</view> -->
		<!-- group_id: 3
		long: day -->
		<hFormAlert v-if="cancelShow" title=""  name="cancel_desc" placeholder="请输入卡密卡号" @confirm="confirm" @cancel="cancel"></hFormAlert>
		<dialog-couponshare v-if="showCouponInvite" @closed="closedInvite"></dialog-couponshare>
	</view>
</template>

<script>
	import hFormAlert from '@/components/h-form-alert/h-form-alert.vue';
	export default{
		components: {hFormAlert},
		data(){
			return {
				num:'',
				moneyList:[],
				numa:'',
				userinfo:'',
				paysj:'',
				showCouponInvite:false,
				cancelShow:false,
			}
		},
		computed:{
			getCoupon(){
				for(var  a in this.moneyList){
					if(this.moneyList[a].num == this.num){
						return this.moneyList[a].coupon;
					}
				}
				return 0;
			}
		},
		onLoad(){
			this.paysja()
		},
		onShareAppMessage(e){
			
		},
		onShareTimeline(e){
			
		},
		methods:{
			changeNum(e){
				console.log(e);
				this.num = e.detail.value;
			},
			async paysja(){
				//this.showCouponInvite = true;
				let data = {};
				let [err,res] =await this.$httpas.get('api.php/User/upgrade',data,{token:true});
				if (!this.$httpas.errorCheck(err,res)) return;
				this.moneyList =res.data
			},
			async paySuccess(group_id,total,long){
				let data = {};
				data.group_id=group_id
				data.long=long
				let [err,res] =await this.$httpas.post('api.php/User/upgrade',data,{token:true});
				if (!this.$httpas.errorCheck(err,res)) return;
				console.log(res.data.code)
				if(res.data.code==1){
					uni.showModal({
						title: '温馨提示',
						content: res.data.msg,
						showCancel: false,
						confirmText: "确定",
						success: function (res) {
							if (res.confirm) {
								uni.navigateBack();
							} else if (res.cancel) {
								
							}
						}
					});
				}else if(res.data.code==1005){
					uni.showModal({
						title: '温馨提示',
						content: res.data.msg+'，请充值',
						showCancel: true,
						confirmText: "充值",
						success: function (res) {
							if (res.confirm) {
								var urlsa='/pages/client/vipcard/recharge?price='+total
								uni.navigateTo({
									url:urlsa
								})
							} else if (res.cancel) {
								
							}
						}
					});
				}else{
					uni.showModal({
						title: '提醒',
						content: JSON.stringify(data),
						showCancel: false,
						confirmText: "确定",
						success: function (res) {
							if (res.confirm) {
								//uni.navigateBack();
							} else if (res.cancel) {
								
							}
						}
					});
				}
			},
			closedInvite(){
				this.showCouponInvite = false;
				let pages = getCurrentPages();
				uni.navigateBack({
					delta:pages.length
				});
			}
		}
	}
</script>

<style>
	.tag-coupon{
		width: 100%;
		height: 80rpx;
		border-radius: 16rpx;
		text-align: center;
		line-height: 80rpx;
		font-size: 28rpx;
	}
	.recharge-header{
		height: 240rpx;
		width: 100%;
		position: relative;
	}
	.recharge-header image{
		width: 100%;
		height: 240rpx;
	}
	.recharge-header .main{
		position: absolute;
		width: 100%;
		height: 240rpx;
		left: 0;
		top: 0;
	}
	.tdadf{
		border: 1px solid #f8f8f8; padding: 10px 5px; background: #ffffff; border-radius: 8px; box-shadow: 0px 4px 20px 0px rgba(0,0,0,0.04);
	}
</style>