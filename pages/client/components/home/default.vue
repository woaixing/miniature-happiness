<template>
	<view>
		<view class="pd16_15 mt15" style="margin-bottom: 80upx;">	
			<view class="flex alcenter space" style="margin-bottom: 10upx;">
				<view class="flex alcenter">
					<image style="width: 40rpx;height: 40rpx;" :src="statics.zhuico[3]"></image>
					<text class="ft16 ftw600 cl-main ml15">热播推荐</text>
				</view>
				<navigator url="/pages/client/tuan/ss">
				<view class="ft14 cl-notice">更多</view>
				</navigator>
			</view>
			<view class="flex space" style="display:flex; flex-wrap:wrap;">
				<block v-for="(value,key) in datasa" :key="key">
				<view class="mt10" style="width: 49%; position: relative;">
					<view class="box"  @click="detail(value.vod_id)">
						<view class="btntpsdz">{{value.type_name}}</view>
						<image class="tuan-product-l" mode="aspectFill" :src="value.vod_pic?value.vod_pic:value.vod_pic"></image>
					</view>
					<view class="mt8 ft14 ftw500 text-over cl-main" style="text-align: left;">{{value.vod_name}}</view>
					<view class="mt5 ft12 ftw400 text-over cl-main" style="text-align: left; color: #999;">{{value.vod_blurb}}</view>
				</view>
				</block>
			</view>
		</view>
		<view class="home-main">
			<com-copyright></com-copyright>
		</view>
		<dialog-birthday v-if="showBirthday" @closed="showBirthday = false"></dialog-birthday>
		<dialog-login v-if="showLogin" @loginYes="loginYes" @closed="showLogin = false"></dialog-login>
		<dialog-qrcode v-if="showQrcode" @closed="showQrcode = false"></dialog-qrcode>
		<dialog-couponshareget @loginAct="showLoginCouponShareGet" v-if="showCouponShareGet" @closed="showCouponShareGet = false"></dialog-couponshareget>
	</view>
</template>

<script>
	export default{
		props: ['dataarr','type'],
		data(){
			return {
				datasa:[],
				isLogin:false,
				showBirthday:false,
				showLogin:false,
				showQrcode:false,
				showCouponShareGet:false,
			}
		},

		created(){
			this.getrm();
			// uni.setNavigationBarTitle({
			//     title: '夜趣小视频'
			// });
			
			// setTimeout(()=>{
			// 	this.showCouponShareGet = true;	
			// },1000);
		},
		methods:{
			getrm() {
				let this_=this
				let data = {};
				data.page=1;
				data.limit=10;
				data.level=1;//推荐一
				if(this.type){
					data.type=this.type;
				}
				data.user_random = uni.getStorageSync("userinfo").user_random;
				uni.request({
					url: this.configs.Url+'/api.php/User/vods',
					data: data,
					success: data => {
						//console.log(data.data.list);
						if (data.data.list) {
							this.datasa=data.data.list
						}
					},
					fail: (data, code) => {
					}
				});
			},
			//'/pages/client/tuan/detail?id='+value.id
			detail(id){
				// #ifndef H5
				uni.redirectTo({
					url:'/pages/client/tuan/detail?id='+id
				})
				// #endif
				// #ifdef H5
				uni.redirectTo({
					url:'/pages/client/tuan/detail?id='+id
				})
				// #endif
			},
			showLoginAct(){
				this.showLogin = true;
			},
			showLoginCouponShareGet(){
				this.showLogin = true;
			},
			loginYes(){
				
			}
		}
	}
</script>

<style>
	.home-header{
		height: 304rpx;
		width: 100%;
		position: relative;
		background: #363B4D;
		border-radius: 0rpx 0rpx 48rpx 48rpx;
	}
	.home-main{
		width: 100%;
		position: relative;
		margin-top: -156rpx;
		padding: 0 30rpx;
	}
	.home-mendian{
		width: 100%;
		height: 84rpx;
		background:rgba(255,255,255,0.1);
		border-radius: 42rpx;
	}
	.tuan-product-l{
		width: 100%;
		height: 240rpx;
		border-radius: 16rpx;
		background: #F2F2F2;
	}
	.tuan-product-r{
		/* width: calc(100% - 320rpx); */
	}
	
</style>
