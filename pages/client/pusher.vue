<template>
	<view class="content">
		<yyqLivePusher></yyqLivePusher>
	</view>
</template>

<script>
 import yyqLivePusher from '@/components/yyq-LivePusher/yyq-LivePusher.vue'
	export default {
		data() {
			return {
				
			}
		},
		onLoad() {
			//plus.navigator.setFullscreen(true); 	
		},
		onShow() {
			//plus.navigator.setFullscreen(true); 
		},
		onHide() {
			plus.navigator.setFullscreen(false); 
		},
		onUnload() {
			plus.navigator.setFullscreen(false); 
		},
		methods: {
			
		},
		components:{
			yyqLivePusher
		}
	}
</script>

<style scoped>
.content {
	background: #000;
	overflow: hidden;
	width: 750upx;
	position: fixed;

}
</style>
