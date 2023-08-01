<template>
	<view class="pd16_15">
		<block v-for="(item,index) in listData" :key="index">
		<view class="box pd16_15 flex alcenter space" style="margin-bottom: 30upx;">
			<view>
				<view class="ft14 cl-main">{{item.plog_type}}</view>
				<view class="mt8 ft12 cl-notice">{{item.plog_time}}</view>
				<!-- <view class="mt8">
					<text class="ft12 cl-notice">备注：</text>
					<text class="ft12 cl-main">暂无</text>
				</view> -->
			</view>
			
			<view class="cl-main ft18 ftw600">
				{{item.plx}}{{item.plog_points}}
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
		data(){
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
		methods:{
			getList() {
				let data = {};
				data.page=this.last_id;
				data.user_random = uni.getStorageSync("userinfo").user_random;
				uni.request({
					url: this.configs.Url+'api.php/User/plog',
					data: data,
					success: data => {
						console.log(data.data)
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
	
</style>