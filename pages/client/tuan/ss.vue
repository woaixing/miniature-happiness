<template>
	<view>
		<sub-tabnavin :tabs="tabs" @change="changeIndex" :selectIndex="selectIndex" :scrollTop="scrollTop"></sub-tabnavin>
		<!-- #ifdef MP-WEIXIN -->
		<view class="" style="background: #ffffff;"> 
			<uni-search-bar @confirm="search" :focus="false" placeholder="请输入搜索内容" v-model="searchValue" @blur="blur" @focus="focus" @input="input"
							@cancel="cancel" @clear="clear">
			</uni-search-bar>
		</view>
		<!-- #endif -->
		<view v-if="selectIndex==0" class="mt15" style="margin:30upx 30upx 0 30upx;box-shadow: 0px 4px 20px 0px rgba(0,0,0,0.04);">
			<home-banner :banners="datahdp"></home-banner>
		</view>
		<sub-tabvs class="pt5" v-if="tabsclass[1]" :tabs="tabsclass" @change="changetype" :selectIndex="selecttype" :scrollTop="scrollTop"></sub-tabvs>
		<sub-tabvs class="pt5" v-if="tabsarea[1]" :tabs="tabsarea" @change="changeadddd" :selectIndex="selectadddd" :scrollTop="scrollTop"></sub-tabvs>
		<sub-tabvs class="pt5" v-if="tabsyear[1]" :tabs="tabsyear" @change="changeyear" :selectIndex="selectyear" :scrollTop="scrollTop"></sub-tabvs>
	
		<view class="pd10_15 flex space" style="display:flex; flex-wrap:wrap;">
			<block v-for="(value,key) in listData" :key="key">
			<view class="pic-item" @click="detail(value.vod_id)" style="position: relative;">
				<view>
					<image mode="aspectFill" class="tuan-product-l" :src="value.vod_pic"></image>
					<view class="btntpsdz">{{value.type_name}}</view>
				</view>
				<view class="mt8 ft14 ftw500 text-over cl-main" style="text-align: left;">{{value.vod_name}}</view>
				<view class="mt5 ft12 ftw400 text-over cl-main" style="text-align: left; color: #999;">{{value.vod_blurb}}</view>
			</view>
			</block>
			<!-- <uni-pagination title="" @change="onPageChange" v-if="total>pageSize" show-icon="true" :pageSize="pageSize" :total="total" current="1"></uni-pagination> -->
		</view>
		<uni-load-more :status="status" :content-text="contentText" />
		<!-- <dialog-login v-if="showLogin" @loginYes="loginYes" @closed="showLogin = false"></dialog-login> -->
		<!-- <com-footer model="tuan"></com-footer> -->
		
	</view>
</template>

<script>
	import uniLoadMore from '@/components/uni-load-more/uni-load-more.vue';
	  export default {
		  components: {
		  	uniLoadMore
		  },
		data(){
			return {
				banners:[],
				vipLevel:0,
				isLogin:true,
				showLogin:false,
				datasa:[],
				dataconfig:[],
				searchValue:'',
				
				tabs:[],
				tabsclass:[],
				selecttype:0,
				ssclass:'',
				
				tabsarea:[],
				tabsyear:[],
				
				selectIndex:0,
				scrollTop:0,
				page:1,
				datahdp:[],
				type_id:0,
				tabname:'精选',
				tabsqa:[],
				
				type2tab:[],
				selecttype2:0,
				type2id:'',
				
				type3tab:[],
				selecttype3:0,
				type3id:'',
				
				type4tab:[],
				selecttype4:0,
				type4id:'',
				
				typetab:[],
				
				
				addddtab:[],
				selectadddd:0,
				addddid:'',
				
				yeartab:[],
				selectyear:0,
				yearid:'',
				
				type:'',
				keytext:'',
				listData: [],
				total:0,
				pageSize:15,
				last_id: 0,
				reload: true,
				status: 'more',
				eselectIndex:0,
				contentText: {
					contentdown: '上拉加载更多',
					contentrefresh: '加载中',
					contentnomore: '没有数据了'
				}
			}
		},
		onLoad(e){
			//console.log(111)
			if(e.selectIndex){
				this.eselectIndex=1
				this.selectIndex=e.selectIndex*1
			}
			if(e.selecttype){
				this.selecttype=e.selecttype*1
			}
			this.status = 'more';
			this.gethdp();
			this.banners=uni.getStorageSync("config").banner
		},
		onPageScroll(e){
			this.scrollTop = e.scrollTop;
		},
		onReachBottom() {
			this.status = '';
			this.getzx();
		},
		//监听搜索框文本变化
		onNavigationBarSearchInputChanged(e) {
			let text = e.text;
			if(text){
				this.keytext=text;	
			}
			console.log(text)
		},
		//监听点击搜索按钮事件
		onNavigationBarSearchInputConfirmed(e) {
			// #ifdef APP-PLUS
			plus.key.hideSoftKeybord();
			// #endif
			var this_=this
			let text = e.text;
			if (!text) {
				uni.showModal({
					title: '温馨提醒',
					content: '请输入搜索内容',
					showCancel: true,
					confirmText: "确定",
					success: function (res) {
						if (res.confirm) {
							this_.keytext='';
							this_.listData=[]
							this_.page=1,
							this_.getzx()
						} else if (res.cancel) {
							this_.keytext='';
							this_.listData=[]
							this_.page=1,
							this_.getzx()
						}
					}
				});
				return;
			} else {
				this.keytext=text;
				this.listData=[];
				this_.page=1,
				this.getzx()
			}
			
		},
		methods:{
			detail(id){
				uni.navigateTo({
					url:'/pages/client/tuan/detail?id='+id
				})
			},
			onPageChange(e) {
			    this.last_id=0
			    this.reload=true
			    this.getList()
				uni.pageScrollTo({
				    scrollTop: 0,
				    duration: 100
				});
			},
			
			getzx() {
				this.status = 'loading';
				let this_=this
				let data = {};
				data.page=this.page;
				data.limit=this.pageSize;
				if(this.keytext){
					data.keytext=this.keytext
				}
				if(this.tabs.length>0){
					this.type_id=this.tabs[this.selectIndex].type_id
				}
				if(this.type_id>0){
					data.type=this.type_id;
				}
				if(this.ssclass!='全部'){
					data.ssclass=this.ssclass;
				}
				if(this.addddid!='全部'){
					data.addddid=this.addddid;
				}
				if(this.yearid!='全部'){
					data.yearid=this.yearid;
				}
				console.log(this.type_id)
				data.user_random = uni.getStorageSync("userinfo").user_random;
				uni.request({
					url: this.configs.Url+'/api.php/User/vods',
					data: data,
					success: data => {
						if (data.data.list.length>0) {
							this.listData=this.listData.concat(data.data.list);
							this.page = this.page+1;
							if(data.data.total<=this.pageSize){
								this.status='noMore'
							}
						}else{
							this.status='noMore'
						}
					},
					fail: (data, code) => {
					}
				});
			},
			gethdp() {
				let this_=this
				let data = {};
				data.page=1;
				data.limit=5;
				data.level=9;//幻灯片
				if(this.selectIndex>0){
					this.type_id=this.selectIndex
				}
				console.log(this.type_id);
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
						if(this.eselectIndex==1){
							this.type_id=this.tabs[this.selectIndex].type_id
							this.tabsadq()
							this.eselectIndex=0
						}
						this.getzx();
					},
					fail: (data, code) => {
					}
				});
			},
			changeIndex(index){
				this.selecttype = 0;
				this.ssclass=this.tabsclass[0]
				this.selectadddd = 0;
				this.addddid=this.tabsarea[0]
				this.selectyear = 0;
				this.yearid=this.tabsyear[0]
				
				this.selectIndex = index;
				this.type_id=this.tabs[index].type_id
				this.tabname=this.tabs[index].name
				this.tabsadq();
				this.listData=[]
				this.page=1,
				this.getzx();			
			},
			tabsadq(){
				this.tabsclass=[]
				this.tabsarea=[]
				this.tabsyear=[]
				for(var i = 0;i<this.tabs.length;i++){
					if(this.tabs[i].type_id==this.type_id){
						this.tabsclass = this.tabsclass.concat(this.tabs[i].type_extend_class);
						this.tabsarea = this.tabsarea.concat(this.tabs[i].type_extend_area);
						this.tabsyear = this.tabsyear.concat(this.tabs[i].type_extend_year);
					}
				}
				this.ssclass=this.tabsclass[this.selecttype]
			},
			changetype(index){
				this.selecttype = index;
				this.ssclass=this.tabsclass[index]
				console.log(this.ssclass);
				this.listData=[]
				this.page=1,
				this.getzx()				
			},
			changeadddd(index){
				this.selectadddd = index;
				this.addddid=this.tabsarea[index]
				console.log(this.addddid);
				this.listData=[]
				this.page=1,
				this.getzx()			
			},
			changeyear(index){
				this.selectyear = index;
				this.yearid=this.tabsyear[index]
				console.log(this.yearid);
				this.listData=[]
				this.page=1,
				this.getzx()				
			},
			
			changetype2(index){
				this.selecttype2 = index;
				this.type2id=this.type2tab[index].id
				this.getdata()				
			},
			changetype3(index){
				this.selecttype3 = index;
				this.type3id=this.type3tab[index].id
				this.getdata()				
			},
			changetype4(index){
				this.selecttype4 = index;
				this.type4id=this.type4tab[index].id
				this.getdata()				
			},
			getdata(){
				this.last_id=0
				this.reload=true
				this.getList()
			},
			loginYes(){
				
			},
			mlinkTo(e){
				if(this.isLogin == true){
					let link = e.currentTarget.dataset.link;
				}else{
					this.showLogin = true;
				}
			},
			search(res) {
				this.listData=[];
				this.last_id=0;
				this.keytext=res.value;
				this.type='';	
				this.getList(0)
			},
	
			clear(res) {
			
			},
			input(res) {
				console.log('----input:', res)
			},
			blur(res) { 
				// this.listData=[];
				// this.last_id=0;
				// this.keytext=res.value;
				// this.type='';	
				// this.getList(0)
				//res.value 
			},
			focus(e) {

			},
			cancel(res) {
				this.keytext='';
				this.type='';	
				this.getList(0)
			}
		}
	}
</script>

<style>

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
	.pic-item{
		display:flex;
		flex-direction: column;
		width: 32%;
		margin-bottom: 20upx;
	}

	.tuan-product-l{
		width: 100%;
		height: 350rpx;
		background: #F2F2F2;
		border-radius: 16upx;
	}
	
	.search-result {
			padding-top: 10px;
			padding-bottom: 20px;
			text-align: center;
		}
	
		.search-result-text {
			text-align: center;
			font-size: 14px;
			color:#666;
		}
	
		.example-body {
			/* #ifndef APP-NVUE */
			display: block;
			/* #endif */
			padding: 0px;
		}
	
		.uni-mt-10 {
			margin-top: 10px;
		}
</style>