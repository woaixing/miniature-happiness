<template>
	<view class="pd16_15">
		<block v-for="(item,index) in listData" :key="index">
		<view class="box pd16_15 alcenter space" style="margin-bottom: 30upx;">
			<view class="flex" style="width: 100%;">
				<view style="width: 70%;">
					<view class="ft14 cl-main">提现姓名：{{item.cash_payee_name}} </view>
					<view class="mt8 ft12 cl-notice">帐号：{{item.cash_bank_no}}</view>
					<view class="mt8">
						<!-- <text class="ft12 cl-notice">手续费：</text>
						<text class="ft12 cl-main">{{item.sxf}}</text> -->
						<text class="ft12 cl-main">{{item.cash_bank_name}}</text>
					</view> 
				</view>
				<view class="uni-triplex-right" style="width: 30%; text-align: right; line-height: 40upx;">
					<text class="uni-h5" style="width: 100%; display: block; font-size: 24upx;">{{item.cash_times}}</text>
					<text class="uni-h5 mt8" style="font-size: 24upx; color: #ff0000; width: 100%; display: block;">￥{{item.cash_money}}</text>
					<text class="uni-h5 mt8" style="width: 100%; display: block; font-size: 24upx;" v-if="item.cash_status==1">已审核</text>
					<text class="uni-h5 mt8" style="width: 100%; display: block;  font-size: 24upx;" v-else>未审核</text>
				</view>
			</view>
			<view v-if="item.memoj" class="mt8" style="width: 100%;">
				<text class="ft12 cl-notice">备注：</text>
				<text class="ft12 cl-main">{{item.memoj}}</text>
			</view>
		</view>
		</block>	
		<uni-load-more :status="status" :content-text="contentText" />
	</view>
</template>
<script>
	import uniLoadMore from '@/components/uni-load-more/uni-load-more.vue';
	  export default {
		  components: {
		  	uniLoadMore
		  },
	    data() {
	        return {
	            listData: [],
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
	    onReachBottom() {
	    	var this_=this
	    	this.status = 'loading';
	    	setTimeout(function() {
	    	    this_.getList();
	    	},500)
	    },
	    onLoad() {
	    	this.getList();
	    },
		methods: {
			getList() {
				let data = {};
				data.page=this.last_id;		
				data.user_random = uni.getStorageSync("userinfo").user_random;
				uni.request({
					url: this.configs.Url+'api.php/User/cash',
					data: data,
					success: data => {
						//console.log(data.data)
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
						//console.log('fail' + JSON.stringify(data));
					}
				});
			},
		}
	}
</script>

<style>
	.uni-title{
		color: #444;
		font-size: 32upx;
		font-weight: normal;
	}
	.uni-text{
		font-size: 28upx;
	}
	.uni-h5{
		font-size: 32upx;
		color: #3a3a3a;
		font-weight:500;
	}
</style>
