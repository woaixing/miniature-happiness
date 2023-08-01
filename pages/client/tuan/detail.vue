<template>
	<view>
		<view class="tuan-detail-header">
			<!-- #ifndef H5 -->
			<video  id="myVideo"
			:src="playindex"
			:poster="info.vod_pic_slide?info.vod_pic_slide:info.vod_pic" 
			:show-center-play-btn="playbtn" 
			:autoplay="autoplay"  style="width: 100%;" 
			@timeupdate="timeupdate" >
			</video>
			<!-- #endif -->
			<!-- #ifdef H5 -->
			<view class="video-js" ref="video" style="width: 100%; height: 100%;">
				
			</view>
			<image v-if="isqxs" :src="info.vod_pic_slide?info.vod_pic_slide:info.vod_pic" mode="aspectFill" style="width: 100%; height: 430upx;"></image>
			<!-- #endif -->
		</view>
		<view class="tuan-detail-tit pd10_15 mb10">
			<view class="flex alcenter space mt12" style="display:flex; flex-wrap:wrap;">
				<view class="ft18 cl-main ftw600" style="width: 70%;">{{info.vod_name}}</view>
				<view @click="shoucang(info)" class="form-footer-item text-center flex space" >
					<view v-if="isshoucang==0" class="ft14 ml5 flex space" style="background: rgb(0, 198, 87); color: #ffffff; border-radius: 5px; padding: 8px 8px;">
						<view class="yticon icon-shoucang ft16" style="color: #ffffff;"></view>
						<view class="ft14">收藏</view>
					</view>
					<view v-else class="ft14 ml5 flex space" style="background: #ff0000; color: #ffffff; border-radius: 5px; padding: 8px 8px;">
						<view class="yticon icon-shoucang ft16" style="color: #ffffff;"></view>
						<view class="ft14">已收藏</view>
					</view>
					
				</view>
			</view>
			<view class="flex alcenter space mt12" style="display:flex; flex-wrap:wrap;">
				<view class="flex alcenter">
					<text class="ft14 cl-orange">价格：</text>
					<text class="ft16 cl-orange ftw600">¥{{info.vod_points}}</text>
				</view>
				
				<view class="cl-notice ft12">{{info.vod_time}}</view>
			</view>
		
		</view>
		<view v-if="info.playcount>1" class="tuan-detail-tit pd16_15 flex space" style="display:flex; flex-wrap:wrap;">
			<view class="ft16 ftw600">选集</view>
			<view @click="opxj()" class="ft14" style="color: #999;">共{{info.playcount}}集 ></view>
		</view>
		<view class="row pl15 pr15" v-if="info.playcount>1"  style=" background: #FFFFFF;">
			<scroll-view  :scroll-with-animation="true" :scroll-x="true" class="tab-nav-plus-scroll">
				<view v-for="(item,index) in info.playurl" :key="index"  :data-index="index" @click="op(item.k)" class="item">
					<view class="tit" :class="mid == item.k ? 'on' : ''" :style="{color: mid == item.k ? '#ffffff': '#353535'}">{{item.name}}</view>
					<!-- <view class="bd" :style="{background:mid == item.k ? tempColor: 'transparent'}"></view> -->
				</view>
			</scroll-view>
		</view>
		<home-default :datasa="datasa"></home-default>
		<view class="form-footer-h">
			<view class="form-footer-h form-footer">
				<view class="form-footer-main pd10_15 flex alcenter space">
						<view class="flex alcenter space pr15" style="width: 100%;">
							<navigator open-type="reLaunch" url="/pages/client/index">
								<view class="form-footer-item text-center">
									<view class="iconfont iconicon_bottom_home ft22"></view>
									<view class="ft12 mt8">首页</view>
								</view>
							</navigator>
							
							<button @click="wdfxm" class="form-footer-item text-center ">
								<view class="iconfont iconicon_data01 ft22"></view>
								<view class="ft12 mt8">分享</view>
							</button>
							<button @click="vipcard" class="form-footer-item text-center ">
								<view class="iconfont icontabbar01 ft22"></view>
								<view class="ft12 mt8">VIP</view>
							</button>
						</view>
						<button v-if="popedom.code==1"  class="btn-big ft14" style="width: 1200rpx; color: #ffffff; background: #C0C0C0!important;">{{isplaytext}}</button>
						<button v-if="popedom.code==3003" @click="getpopedom(popedom)" class="btn-big ft14" style="width: 1200rpx;" :style="getBtnStyle">支付￥{{info.vod_points}}</button>
						<button v-if="popedom.code==3001" @click="getpopedom(popedom)" class="btn-big ft14" style="width: 1200rpx;" :style="getBtnStyle">升级VIP会员</button>
						<button v-if="popedom.code==20001" @click="getpopedom(popedom)" class="btn-big ft14" style="width: 1200rpx;" :style="getBtnStyle">请登录</button>	
				</view>
			</view>
		</view>
		<!-- <view class="xuanji">
			
		</view> -->
	</view>
</template>

<script>
import zaudio from '@/components/uniapp-zaudio/zaudio';
// import zaudio from 'uniapp-zaudio/zaudio'
export default {
	components: { zaudio: zaudio },
		data(){
			return {
				videoPlay:null,
				isLogin:false,
				showxuanji:false,
				autoplay:false,
				playbtn:true,
				selectIndex:0,
				tabs:['详情'],
				nextStep:'',
				datasa:[],
				dataconfig:[],
				id:'',
				mid:0,
				isqxs:false,
				info:[],
				dda:'',
				price:'',
				banners:[],
				isdj:1,
				playindex:'',
				popedom:[],
				isplaytext:'当前视频免费观看',
				isshoucang:0,
				audioAction: {
				    method: 'pause'
				}
			}
		},  
		onUnload(){
			// #ifdef H5
			if (this.videoPlay) {
			    this.videoPlay.dispose();
			}
			// #endif    
		},
		onLoad(option){
			
			if(option.fxid){
				uni.setStorage({
					key: 'fxid',  
					data: option.fxid
				})
			}
			this.dda=uni.createVideoContext('myVideo')
			if(option.id){
				this.id=option.id
			}
			if(option.mid){
				this.mid=option.mid
			}
			if(option.autoplay){
				this.autoplay=true
			}
			this.getinfo(this.id);
		},
		onShow() {
			
		},
		onHide() {
			
		},
		onShareAppMessage(res) {
			var this_=this
		    if (res.from === 'menu') {// 来自页面内分享按钮
		      console.log(res.target)
		    }
		    return {
		      title: '6DB影视',
			  channel:true,
		      path: '/pages/client/index?fxid='+this_.uid
		    }
		},
		onReady() {  
		              
		        },  
		methods:{
			opxj(){
				this.showxuanji=true
			},
			timeupdate(event){
				
					let _this=this;
					let currentTime = event.detail.currentTime 
					if(currentTime>this.info.mrseek && this.info.isplay==0){
						 //dda.exitFullScreen()
						 this.dda.seek(this.info.mrseek)
						 this.dda.pause()
						 this.dda.stop()
						 uni.showModal({
							title: '温馨提示',
							content: '没权限观看当前视频',
							showCancel: false,
							confirmText: "确定",
							success: function (res) {
								if (res.confirm) {
									
								} else if (res.cancel) {
									
								}
							}
						 });
					}
			},
			showLoginAct(){
				uni.navigateTo({
					url:'/pages/login/login'
				})
			},
			loginYes(){
				
			},
			shoucang(info){
				var this_=this
				if(this.popedom.code==20001){
					uni.showModal({
						title: '温馨提示',
						content: '当前未登录 请登录',
						showCancel: true,
						confirmText: "确定",
						success: function (res) {
							if (res.confirm) {
								uni.redirectTo({
									url:'/pages/login/login'
								})
							} else if (res.cancel) {
														
							}
						}
					});
					return false;
				}
				var nid=this.mid*1+1
				let data = {'id':this.id,'nid':nid};
				data.user_random = uni.getStorageSync("userinfo").user_random;
				uni.request({
					url: this.configs.Url+'api.php/User/shoucang',
					data: data,
					success: resa => {
						uni.showModal({
							title: '提示',
							content: resa.data.msg,
							showCancel: false,
							success: res => {
								this_.isshoucang=resa.data.isshoucang
								//uni.navigateBack();	
							}
						});
					},
					fail: (data, code) => {
							
					}
					});
			},
			wdfxm(){
				var shareurl=this.info.vod_pic?this.info.vod_pic:this.info.vod_pic
				uni.setStorage({//缓存配置信息
					key: 'shareurl',  
					data: shareurl
				})
				uni.navigateTo({
					url:'/pages/client/member/qrshare?vod_id='+this.info.vod_id
				})
			},
			getinfo(id) {
				let data = {};
				data.id=id
				data.mid=this.mid
				data.user_random = uni.getStorageSync("userinfo").user_random;
				uni.request({
					url: this.configs.Url+'api.php/User/play',
					data: data,
					success: data => {
							console.log(data.data);
							this.info=data.data
							this.isshoucang=data.data.isshoucang
							this.popedom=data.data.popedom
							uni.setNavigationBarTitle({
							    title: this.info.vod_name
							});
							if(data.data.popedom.code==1){
								this.playindex=this.info.playindex.url
								// // #ifdef H5
								// var video = document.createElement('video')
								// video.id = 'video'
								// video.controls = true
								// var source = document.createElement('source')
								// source.src =this.info.playindex.url
								// video.appendChild(source)
								// this.$refs.video.$el.appendChild(video)
								// // videojs('video')
								// // video.poster = this.info.vod_pic?this.info.vod_pic:'';
								// video.style = 'width: 100%;';
								// video.autoplay= this.autoplay
								// // this.videoPlay = videojs('video');
								// // #endif
								
								// #ifdef H5
								var video = document.createElement('video')  
								video.id = 'video'
								
								video.style = 'width: 100%; height: 300px;'  
								video.controls = true  
								var source = document.createElement('source')  
								source.src = this.info.playindex.url  
								video.appendChild(source)  
								this.$refs.video.$el.appendChild(video)  
								video.autoplay= this.autoplay
								video.poster = this.info.vod_pic?this.info.vod_pic:'';
								videojs('video')
								this.videoPlay = videojs('video');
								// #endif
							}else{
								this.isqxs=true
								this.getpopedom(data.data.popedom)
								return false;
							}
							
					},
					fail: (data, code) => {
					}
				});
			},
			getpopedom(popedom){
				var this_=this
				uni.showModal({
					title: '温馨提示',
					content: popedom.msg +'【'+ popedom.code+'】',
					showCancel: true,
					confirmText: "确定",
					success: function (res) {
						if (res.confirm) {
							if(popedom.code==20001){
								uni.redirectTo({
									url:'/pages/login/login'
								})
							}else if(popedom.code==3003){
								//uni.showToast({ title: '1111',icon:"none" });
								this_.buypopedom()
							}else{
								uni.redirectTo({
									url:'/pages/client/vipcard/index'
								})
							}
						} else if (res.cancel) {
							
						}
					}
				});
			},
			async buypopedom(){
				var this_=this
				let data = {'id':this.id,'nid':this.mid+1};
				let [err,res] =await this.$httpas.post('api.php/User/ajax_buy_popedom',data,{token:true});
				if (!this.$httpas.errorCheck(err,res)) return;
				//console.log(res.data.data)
				if(res.data.code == 1){
					uni.showModal({
						title: '温馨提示',
						content: '支付成功',
						showCancel: true,
						confirmText: "确定",
						success: function (res) {
							if (res.confirm) {
								this_.isqxs=false
								this_.getinfo(this_.id);
							} else if (res.cancel) {
								
							}
						}
					});
				}else if(res.data.code == 2002){
					uni.showModal({
						title: '温馨提示',
						content: '积分不足 请充值',
						showCancel: true,
						confirmText: "确定",
						success: function (res) {
							if (res.confirm) {
								uni.navigateTo({
									url:'/pages/client/vipcard/recharge'
								})
							} else if (res.cancel) {
								
							}
						}
					});
				}
			},
			contactAct(){
				if(this.isLogin == false){
					this.showLogin = true;
					this.nextStep = 'contact';
				}else{
					uni.navigateTo({
						url:'/pages/client/vipcard/adviser'
					})
				}
			},
			vipcard(){
				uni.navigateTo({
					url:'/pages/client/vipcard/index'
				})
			},
			op(mid){
				uni.redirectTo({
					url:'/pages/client/tuan/detail?id='+this.id+'&mid='+mid+'&autoplay='+1
				})
				
			},
			buyAct(){
				if(this.price*1==0){
					uni.showToast({ title: '免费视频不需要购买',icon:"none" });
					return false; 
				}
				if(this.isLogin == false){
					this.showLogin = true;
					this.nextStep = 'buy';
				}else{
					uni.setStorage({//缓存配置信息
						key: 'buydata',  
						data: this.info
					})
					uni.navigateTo({
						url:'/pages/client/tuan/buy?id='+this.id
					})
				}
			},
			loginYes(){
				if(this.nextStep == 'buy'){
					uni.navigateTo({
						url:'/pages/client/tuan/buy'
					})
				}else{
					uni.navigateTo({
						url:'/pages/client/vipcard/adviser'
					})
				}
			},
			changeIndex(index){
				this.selectIndex = index;
			}
		}
	}
</script>

<style>
	.video-js .vjs-big-play-button{
		top: 50%;
		left: 50%;
	}
	.xuanji{
		height: 50%;
		position: fixed;
		bottom: 0px;
		width: 100%;
		background: #999;
		z-index: 2;
	}
	.user-not-vip{
		width: 100%;
		height: 80rpx;
		background: #FDF6EC;
		border-radius: 40rpx;
		border: 2rpx solid #EFC381;
		text-align: center;
		line-height: 76rpx;
		font-size: 24rpx;
		color:#000000;
		font-weight: bold;
	}
	
	.tuan-detail-header{
		position: relative;
	}
	.tuan-detail-swiper{
		height: 500rpx;
	}
	.tuan-detail-swiper image{
		width: 100%;
		height: 500rpx;
		background: #F2F2F2;
	}
	.tuan-detail-tit{
		width: 100%;
		background: #FFFFFF;
		/* border-radius: 40rpx 40rpx 0rpx 0rpx; */
		position: relative;
	}
	.tuan-detail-content-tab{
		height: 102rpx;
	}
	.tuan-detail-content{
		min-height: calc(100vh - 600rpx);
		background: #FFFFFF;
	}
	.tuan-product-l{
		width: 320rpx;
		height: 240rpx;
		border-radius: 16rpx;
		background: #F2F2F2;
	}
	.tuan-product-r{
		width: calc(100% - 320rpx);
	}
	.cl-orange1{
		margin-right: 10upx;
		padding: 15upx;
		border: 1px solid #CCCCCC;
	}
	.mini-btn{
		margin-left: 10upx;
		margin-bottom: 10upx;
	}
	.tab-nav-plus{
		height: 80rpx;
	}
	.tab-nav-plus-main{
		width: 100%;
		height: 80rpx;
		padding: 14rpx 30rpx 0rpx 32rpx;
		/* background: #FFFFFF; */
		/* border-top: 0rpx solid #E4E6ED; */
	}
	.tab-nav-plus-main.fixed{
		/* position: fixed; */
		left: 0;
		z-index: 20;
		background: #f8f8f8;
		box-shadow:0rpx 4rpx 16rpx 0rpx rgba(0,0,0,0.08);
	}
	.tab-nav-plus-scroll{
		width: 100%;
		white-space: nowrap;
		height: 110rpx;
	}
	
	.tab-nav-plus-scroll .item{
		height: 80rpx;
		line-height: 80rpx;
		display: inline-block;
		position: relative;

	}
	.itemsa{
		height: 80rpx;
		line-height: 80rpx;
		display: inline-block;
		position: relative;
	
	}
	.tab-nav-plus-scroll .item:last-child{
		margin-right: 0;
	}
	.tab-nav-plus-scroll .item .tit{
		text-align: center;
		font-size: 28rpx;
		font-weight: 300;
		padding: 0 30upx;
		border: solid 1upx #ccc;
		margin-right: 10upx;
		border-radius: 10upx;
	}
	.tab-nav-plus-scroll .item .tit.on{
		font-size: 28rpx;
		font-weight: 600;
		background: #ff0000;
		border: solid 1px #ff0000;
	}
	.tab-nav-plus-main .tab-nav-plus-scroll .item .bd{
		position: absolute;
		z-index: 1;
		left: calc(50% - 18rpx);
		bottom: 0;
		width: 36rpx;
		height: 12rpx;
		border-radius: 6rpx 6rpx 0rpx 0rpx;
	}
	
	.tab-nav-plus-main.fixed  .tab-nav-plus-scroll .item .bd{
		background: #FFFFFF;
	}
</style>