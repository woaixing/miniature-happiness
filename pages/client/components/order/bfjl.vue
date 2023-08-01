<template>
	<view>
		<view class="box mt16 pd16_15">
			<view class="order-list-item-tit" style="margin-bottom: 20upx;">
				<view class="flex alcenter space" >
					<view class="flex alcenter">
						<text class="ft14 cl-main">{{datas.out_trade_no}}</text>
					</view>
					<text class="ft14 ftw500" :style="{color:getColor}">{{datas.status_means}}</text>
				</view>
			</view>
			<view class="flex">
				<image mode="aspectFill" class="order-list-item-l" :src="datas.data.pic" style="width: 45%; height: auto; margin-right: 5%; border-radius: 5px;"></image>
				<view class="order-list-item-r">
					<view style="width: 300upx;" class="ft14 ftw600 cl-main text-over">{{datas.data.name}}</view>
					<view class="mt8 ft12 cl-notice">{{datas.ulog_time}}</view>
					<view class="flex alcenter cl-orange mt8" >
							<text class="ft12 cl-notice ">消耗积分：{{datas.ulog_points}}</text>
					</view>
					<view class="flex alcenter space mt10">
						<view class="btn-small plan" @click="ulog_del()" style="width: 60px;">删除</view>
						<view class="btn-small" style="width: 85px;" :style="getBtnStyle">重新观看</view>
					</view>
					
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	//status  0,待付款  1待使用    4已过期  8已完成
	export default{
		props:{
			datas:{
				type:Object,
				default:function(){
					return new Object;
				}
			}
		},
		computed:{
			getColor(){
				if(this.datas.status != 0){
					return  '#000000';
				}
				return this.tempColor;
			}
		},
		data(){
			return {
				
			}
		},
		created(){
			
		},
		methods:{
			
			opuer(sid,smid,splx,lx){
				console.log(splx)
				if(splx==1){ //视频
					uni.navigateTo({
						url:'/pages/client/tuan/detail?id='+sid+'&mid='+smid
					})
				}
				if(splx==2 && lx==1){
					uni.navigateTo({
						url:'/pages/client/tuan/info?id='+sid+'&mid='+smid //音频全集
					})
				}
				if(splx==2 && lx==2){
					uni.navigateTo({
						url:'/pages/client/tuan/audio?id='+sid+'&mid='+smid //音频单集
					})
				}
				if(splx==3 && lx==1){
					uni.navigateTo({
						url:'/pages/client/tuan/info?id='+sid+'&mid='+smid //文字全集
					})
				}
				if(splx==3 && lx==2){
					uni.navigateTo({
						url:'/pages/client/tuan/text?id='+sid+'&mid='+smid //文字单集
					})
				}
				if(splx==4 && lx==1){
					uni.navigateTo({
						url:'/pages/client/tuan/infotp?id='+sid+'&mid='+0 //文字全集
					})
				}
			}
		}
	}
</script>

<style>
	.delivery-type{
		width: 128rpx;
		height: 36rpx;
		background: #FFFFFF;
		border-radius: 18rpx;
		font-size: 24rpx;
		color:#8A8D99;
		line-height: 36rpx;
		text-align: center;
		font-weight: 500;
		margin-left: 0rpx;
	}
	.mt16{
		margin-top: 0px;
	}
</style>