<template>
	<view>
		<swiper class="home-swiper-banner" indicator-color="rgba(255, 255, 255, 0.3)" indicator-active-color="#FFFFFF" :indicator-dots="true" :autoplay="true" :interval="3000" :duration="500">
			<swiper-item v-for="(item,index) in banners" :key="index">
				<image :src="item.vod_pic_slide?item.vod_pic_slide:item.vod_pic" @click="detail(item.vod_id)" mode="aspectFill" style="border-radius: 16rpx"></image>
				
			</swiper-item>
			
		</swiper>
	</view>
</template>

<script>
	export default{
		props: ['banners'],
		data(){
			return {
				banners1:[],
			}
		},
		methods:{
			detail(id){
				uni.navigateTo({
					url:'/pages/client/tuan/detail?id='+id
				})
			},
		}
	}
</script>

<style>
	.home-swiper-banner{
		height: 300rpx;
		box-shadow: 0rpx 4rpx 32rpx 0rpx rgba(0, 0, 0, 0.04);
		border-radius: 16rpx;
		overflow: hidden;
	}
	.home-swiper-banner image{
		width: 100%;
		height:300rpx;
	}
</style>
