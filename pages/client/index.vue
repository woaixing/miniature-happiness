<template>
	<view>
		<!-- 标题栏和状态栏占位符 -->
		<!-- <view class="titleNview-placing"> </view> -->
		<sub-tabnavin :tabs="tabs" @change="changeIndex" :selectIndex="selectIndex" :scrollTop="scrollTop"></sub-tabnavin>
		<view class="mt10 plr15" v-if="datahdp">
			<home-banner :banners="datahdp"></home-banner>
		</view>
		<view class="mt10 mb15" v-if="type_id==0">
			<view class="integal-mall-menu flex pt16 pb16 pl15 pr15" >
				<view class="col2 text-center" @click="linkTo" data-link="/pages/client/tuan/ss?selectIndex=1">
					<view>
						<image style="width: 60rpx; height: 60rpx;" :src="statics.zhuico[0]"></image>
					</view>
					<view class="ft14 ftw500 mt6">热播电影</view>
				</view>
				<view class="col2 text-center bd-left"  @click="linkTo"  data-link="/pages/client/tuan/ss?selectIndex=2">
					<view>
						<image style="width: 60rpx; height: 60rpx;" :src="statics.zhuico[7]"></image>
					</view>
					<view class="ft14 ftw500 mt6">电视剧</view>
				</view>
				<!-- /pages/client/integral/role -->
				<view class="col2 text-center bd-left"  @click="linkTo"  data-link="/pages/client/tuan/ss?selectIndex=4">
					<view>
						<image style="width: 60rpx; height: 60rpx;" :src="statics.zhuico[2]"></image>
					</view>
					<view class="ft14 ftw500 mt6">动漫</view>
				</view>
				<view class="col2 text-center bd-left"  @click="linkTo"  data-link="/pages/client/tuan/ss">
					<view>
						<image style="width: 60rpx; height: 60rpx;" :src="statics.zhuico[5]"></image>
					</view>
					<view class="ft14 ftw500 mt6">最近更新</view>
				</view>
			</view>
		</view>
		<view class="mt10" v-if="type_id>0 && tabsqa[1]">
			<view class="integal-mall-menus flex pb10 pl15 pr15" style="display:flex; flex-wrap:wrap;">
				<block v-for="(value,key) in tabsqa" :key="key" v-if="key<4">
				<view class="col2 text-center mt10" :class="key>1?'bd-left':''" style="width:20%;" @click="linkToss(type_id,value.name,key)" >
					<view>
						<image style=" border-radius: 5px; width: 90rpx; height: 90rpx;" :src="value.type_logo"></image>
					</view>
					<view class="ft14 ftw300 mt6">{{value.name}}</view>
				</view>
				</block>
				<view class="col2 text-center mt10 bd-left" style="width:20%;" @click="linkTossq(type_id)">
					<view>
						<image style=" border-radius: 5px; width: 90rpx; height: 90rpx;" :src="statics.zhuico[8]"></image>
					</view>
					<view class="ft14 ftw300 mt6">全部</view>
				</view>
			</view>
		</view>
		<view class="mt16 plr15 mb15">
			<view class="" v-if="datalist[1]">
				<view class="flex alcenter space">
					<view class="flex alcenter">
						<image style="width: 40rpx;height: 40rpx;" :src="statics.zhuico[0]"></image>
						<text class="ft16 ftw600 cl-main ml15">最新{{tabname}}</text>
					</view>
					<navigator url="/pages/client/tuan/ss?selectIndex=1">
					<view class="ft14 cl-notice">更多</view>
					</navigator>
				</view>
				<view class="mt5 flex space" style="display:flex; flex-wrap:wrap;">
					<block v-for="(value,key) in datalist" :key="key">
					<view class="mt10" style="width: 49%; position: relative;">
						<view class="box"  @click="detail(value.vod_id)">
							<view class="btntpsdz">{{value.type_name}}</view>
							<image class="integral-mall-goods" mode="aspectFill" :src="value.vod_pic_slide?value.vod_pic_slide:value.vod_pic"></image>
						</view>
						<view class="mt8 ft14 ftw500 text-over cl-main" style="text-align: left;">{{value.vod_name}}</view>
						<view class="mt5 ft12 ftw400 text-over cl-main" style="text-align: left; color: #999;">{{value.vod_blurb}}</view>
					</view>
					</block>
				</view>
			</view>
		</view>	
		<!-- #ifdef APP-PLUS -->
		<view class="ad-view">
		    <ad adpid="1111111111" @load="onload" @close="onclose" @error="onerror"></ad>
		</view>
		<!-- #endif -->
		<home-default :dataarr="datasa" :type="selectIndex"></home-default>
		<!-- <com-footer model="index"></com-footer> -->
	</view>
</template>
<script>
	export default{
		data(){
			return {
				tabs:[],
				tabsqa:[],
				selectIndex:0,
				scrollTop:0,
				
				navLock:false,
				datasa:[],
				datalist:[],
				datahdp:[],
				showdyxx:true,
				dataconfig:[],
				banners:[],
				datainfo:[],
				dataindex:[],
				type_id:0,
				tabname:'精选',
				mbgColor:this.$mbgColor,
			}
		},
		computed:{
			
		},
		onPageScroll(e){

		},
		onShareAppMessage(e){
			
		},
		onShareTimeline(e){
			
		},
		onLoad(e){
			uni.setNavigationBarColor({
				frontColor:"#000000",
				backgroundColor:'#f8f8f8',
				complete:()=>{
					this.navLock = false;
				}
			});
			// #ifdef H5
			if (this.videoPlay) {
			    this.videoPlay.dispose();
			}
			// #endif
			this.gethdp();
			this.getzx();
			
		},
		onShow() {
			
			let this_=this
			
		},
		onPullDownRefresh(){
			this.datalist=[]
			this.datahdp=[]
			this.gethdp();
			this.getzx();
			setTimeout(function() {
			    uni.stopPullDownRefresh();
			},500)
		},
		methods:{
			linkToss(id,name,key){
				key=key*1+1
				uni.navigateTo({
					url:'/pages/client/tuan/ss?selectIndex='+id+'&selecttype='+key
				})
			},
			linkTossq(id){
				uni.navigateTo({
					url:'/pages/client/tuan/ss?selectIndex='+id
				})
			},
			changeIndex(index){
				console.log(index)
				this.selectIndex = index;
				this.type_id=this.tabs[index].type_id
				this.tabname=this.tabs[index].name
				this.datalist=[]
				this.datahdp=[]
				this.tabsqa=[]
				for(var i = 0;i<this.tabs.length;i++){
					if(this.tabs[i].type_pid==this.type_id){
						this.tabsqa = this.tabsqa.concat(this.tabs[i]);
					}
				}
				this.gethdp();
				this.getzx();				
			},
			gethdp() {
				let this_=this
				let data = {};
				data.page=1;
				data.limit=5;
				data.level=9;//幻灯片
				if(this.type_id>0){
					data.type=this.type_id;
				}
				data.user_random = uni.getStorageSync("userinfo").user_random;
				uni.request({
					url: this.configs.Url+'/api.php/User/vods',
					data: data,
					success: data => {
						//console.log(data.data.list);
						if (data.data.list) {
							this.datahdp=data.data.list
						}
						this.tabs=data.data.class
					},
					fail: (data, code) => {
					}
				});
			},
			getzx() {
				let this_=this
				let data = {};
				data.page=1;
				data.limit=12;
				if(this.type_id>0){
					data.type=this.type_id;
				}
				data.user_random = uni.getStorageSync("userinfo").user_random;
				uni.request({
					url: this.configs.Url+'api.php/User/vods',
					data: data,
					success: data => {
						//console.log(data.data.list);
						if (data.data.list) {
							this.datalist=data.data.list
						}
					},
					fail: (data, code) => {
					}
				});
			},
			
			
			saoma(){
				//#ifdef APP-PLUS  
				uni.scanCode({
				    success: function (res) {
						if(res.result.indexOf("uid") != -1){
							var obj = JSON.parse(res.result); 
							if(obj.uid){
								uni.navigateTo({
									url:'/pages/login/reg?uid='+obj.uid
								})
							}	
						}else{
							uni.showToast({ title: res.result,icon:"none" });	
						}	
				    }
				});
				//#endif
				
			},
			detail(id){
				uni.navigateTo({
					url:'/pages/client/tuan/detail?id='+id
				})
			},
			linkTo(e){
				if(this.isLogin == false){
					this.showLogin = true;
				}else{
					let link = e.currentTarget.dataset.link;
					uni.navigateTo({
						url:link
					})
				}
			},
			linkToty(e){
				if(this.isLogin == false){
					this.showLogin = true;
				}else{
					let link = e.currentTarget.dataset.link;
					uni.switchTab({
						url:link
					})
				}
			},
			exchange(e){
				if(this.isLogin == false){
					this.showLogin = true;
				}else{
					let id = e.currentTarget.dataset.id;
					uni.navigateTo({
						url:'/pages/client/integral/exchange?id='+id
					})
				}
			},
			onload(e) {
			      console.log("onload");
			    },
			    onclose(e) {
			      console.log("onclose: " + e.detail);
			    },
			    onerror(e) {
			      console.log("onerror: " + e.detail.errCode + " message:: " + e.detail.errMsg);
			    }
		},
		
	}
</script>

<style>
	.home-header{
		height: 200rpx;
		width: 100%;
		position: relative;
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
	.integral-mall-header{
		position: relative;
		height: 320rpx;
	}
	.integral-mall-header .bg{
		width: 100%;
		height: 320rpx;
	}
	.integral-mall-header .main{
		position: absolute;
		left: 0;
		top: 0;
		width: 100%;
		height: 320rpx;
	}
	.swiper-integral{
		height: 32rpx;
		width: 100%;
	}
	.integral-mall-main{
		position: relative;
		margin-top: -104rpx;
	}
	.integal-mall-menu{
		width: 100%;
		height: 176rpx;
		background: #FFFFFF;
		/* border-radius: 20rpx; */
	}
	.integal-mall-menus{
		width: 100%;
		background: #FFFFFF;
		/* border-radius: 20rpx; */
	}
	.integral-tuan-l{
		width: 240rpx;
		height: 180rpx;
		background: #f2f2f2;
		border-radius: 16rpx;
	}
	
	
	.integral-mall-coupon{
		height: 408rpx;
		width: 330rpx;
		background: #FFFFFF;
		position: relative;
		border-radius: 16rpx;
		overflow: hidden;
	}
	.integral-mall-coupon  .top{
		padding: 0rpx 0rpx 24rpx 0rpx;
		border-bottom: 2rpx dashed #FEC675;
	}
	.integral-mall-coupon  .y-l,.integral-mall-coupon  .y-r{
		width: 20rpx;
		height: 20rpx;
		border-radius: 10rpx;
		background: #F5F6FA;
		position: absolute;
		z-index: 2;
		top: 284rpx;
	}
	.integral-mall-coupon  .y-l{
		left: -10rpx;
	}
	.integral-mall-coupon  .y-r{
		right: -10rpx;
	}
	.integral-mall-coupon   .coupon-value{
		width: 100%;
		height: 250rpx;
		position: relative;
	}
	.integral-mall-coupon  .coupon-value image{
		width: 100%;
		height: 180rpx;
	}
	.integral-mall-coupon  .coupon-value .num{
		width: 100%;
		height: 64rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		position: absolute;
		left: 0;
		top: 0;
	}
	.integral-mall-goods{
		width: 100%;
		height: 200rpx;
		background: #F2F2F2;
		border-radius: 16upx;
	}
	.titleNview-placing {
		height: var(--status-bar-height);
		padding-top: 44px;
		box-sizing: content-box;
	}
</style>