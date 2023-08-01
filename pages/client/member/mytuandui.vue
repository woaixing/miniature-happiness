<template>
	<view>
		<sub-tabnavsaq :tabs="tabs" @change="changeIndex" :selectIndex="selectIndex" :scrollTop="scrollTop"></sub-tabnavsaq>
		<view class="pd10_15">
		<view class="order-time-line">
			<view class="order-time-main">
				<view v-for="(item,index) in listData" :key="index" class="pb24">
					<view class="box pd16_15">
						<view class="flex">
							<image mode="aspectFill" class="order-list-item-l" :src="item.user_portrait" style="height: auto; border-radius: 5px;"></image>
							<view class="order-list-item-r">
								<view style="width: 300upx; " class="ft14 ftw600 cl-main text-over">{{item.user_nick_name?item.user_nick_name:item.user_name}}</view>
								<view class="mt8 ft14 cl-notice">ID：{{item.user_id}}</view>
								<view class="flex alcenter cl-orange mt8" >
										<text class="ft14 cl-notice ">{{item.user_reg_time}}</text>
								</view>
							</view>
						</view>
					</view>
				</view>
				<!-- <uni-pagination title="" @change="onPageChange" v-if="total>pageSize" show-icon="true" :pageSize="pageSize" :total="total" current="1"></uni-pagination> -->
			</view>
		</view>
		<uni-load-more :status="status" :content-text="contentText" />
		</view>
	</view>
</template>

<script>
	import uniPagination from '@/components/uni-pagination/uni-pagination.vue';
	import uniLoadMore from '@/components/uni-load-more/uni-load-more.vue';
	export default{
			components: {
			        uniPagination,
					uniLoadMore
			    },
		data(){
			return {
				datasa:[//type 1 次卡 2抢购  3积分商城 exchange_type 1特惠套餐  2 优惠券  3是实物 实物订单多一个字段 delivery_type 1 快递  2 门店自提
					// {type:3,exchange_type:3,delivery_type:1,status:0,status_means:'待支付'},
					// {type:3,exchange_type:3,delivery_type:1,status:1,status_means:'待发货'},
					// {type:3,exchange_type:3,delivery_type:2,status:2,status_means:'待自提'},
					// {type:3,exchange_type:2,status:0,status_means:'待支付'},
					// {type:3,exchange_type:1,status:0,status_means:'待支付'},
					// {type:1,exchange_type:0,status:0,status_means:'待支付'}, // status 各个组件自己定义  积分商城才有兑换的类型
					// {type:2,exchange_type:0,status:0,status_means:'待支付'},
				],
				//datasa:[],
				total:0,
				pageSize:10,
				listData: [],
				tabs:[
					{name:'一级'},
					{name:'二级'},
					{name:'三级'},
				],
				selectIndex:0,
				scrollTop:0,
				
				last_id: 1,
				reload: true,
				status: 'more',
				contentText: {
					contentdown: '上拉加载更多',
					contentrefresh: '加载中',
					contentnomore: '没有数据了'
				}
			}
		},
		onLoad(){
			//console.log(111)
			this.getList()
		},
		onReachBottom() {
			var this_=this
			this.status = 'loading';
			setTimeout(function() {
			    this_.getList();
			},500)
		},
		methods:{
			changeIndex(index){
				this.last_id=1
				this.selectIndex=index
				this.listData = [];
				this.reload = false;
				this.status = '';
				this.getList()
			},
			onPageChange(e) {
			    this.getList(e.current-1)
				uni.pageScrollTo({
				    scrollTop: 0,
				    duration: 100
				});
			},
			getList() {
				let data = {};
				data.page=this.last_id;	
				data.level=this.selectIndex*1+1
				data.user_random = uni.getStorageSync("userinfo").user_random;
				uni.request({
					url: this.configs.Url+'api.php/User/reward',
					data: data,
					success: data => {
						console.log(data.data)
						// this.total=data.data.total
						// this.datasa=data.data.rows
						if (data.data.list) {
							let list = data.data.list;
							this.listData = this.reload ? list : this.listData.concat(list);
							this.reload = false;
							this.last_id = this.last_id+1;
							this.status = '';
						}else{
							this.status = '';
							this.contentText.contentdown='没有数据'
						}
						
					},
					fail: (data, code) => {
					}
				});
			},
			detail(id,mid){
				// /pages/client/tuan/detail?id=1251&mid=0&autoplay=1
				var mid=mid-1
				uni.navigateTo({
					url:'/pages/client/tuan/detail?id='+id+'&mid='+mid+'&autoplay=1'
				})
			},
			ulog_del(ids,type,index){
				var this_=this
				uni.showModal({
					title: '温馨提示',
					content: '是否确认删除',
					showCancel: true,
					confirmText: "确定",
					success: function (res) {
						if (res.confirm) {
							this_.ulog_dels(ids,type,index);
						} else if (res.cancel) {
														
						}
					}
				});
			},
			ulog_dels(ids,type,index){
				var this_=this
				let data = {};
				data.ids =ids;	
				data.type =type;
				data.user_random = uni.getStorageSync("userinfo").user_random;
				uni.request({
					url: this.configs.Url+'api.php/User/ulog_del',
					data: data,
					success: data => {
						uni.showModal({
							title: '温馨提示',
							content: data.data.msg,
							showCancel: false,
							confirmText: "确定",
							success: function (res) {
								if (res.confirm) {
									this_.listData.splice(index,1)
								} else if (res.cancel) {
									
								}
							}
						});
					},
					fail: (data, code) => {
					}
				});
			},
		}
	}
</script>

<style>
	
	.order-time-line{
		padding-left: 0rpx;
	}
	.order-time-main{
		width: 100%;
	}
	.order-list-item-tit{
		position: relative;
		height: 60rpx;
		line-height: 60rpx;
	}
	.order-list-item-tit .order-type{
		width: 60rpx;
		height: 60rpx;
		position: absolute;
		left: -90rpx;
		top: 0;
	}
	.order-list-item-l{
		width: 25%;
		height: 150rpx;
		border-radius: 8rpx;
		background: #F2F2F2;
	}
	.order-list-item-r{
		width: calc(100% - 25%);
		padding-left: 30rpx;
	}
	.pb24 {
	    padding-bottom: 40upx;
	}
</style>