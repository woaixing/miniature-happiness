<template>
	<view class="container">
		<view class="list-cell b-b avatar" hover-class="cell-hover" :hover-stay-time="50"  @click="clk(0)">
			<text class="cell-tit">头像</text>
			<image :src="avatar" mode="aspectFill"></image>
			<text class="cell-more yticon icon-you"></text>
		</view>
		<view class="list-cell" @click="navTo('../../set/grzl/nc/nc?ty=user_nick_name&name=呢称&v='+user_name)" hover-class="cell-hover" :hover-stay-time="50">
			<text class="cell-tit">昵称</text>
			<text class="cell-tip">{{ user_name }}</text>
			<text class="cell-more yticon icon-you"></text>
		</view>
		<view class="list-cell" @click="navTo('../../set/grzl/nc/nc?ty=user_email&name=邮箱&v='+listgrzl.birthday)" hover-class="cell-hover" :hover-stay-time="50">
			<text class="cell-tit">邮箱</text>
			<text class="cell-tip">{{userinfo.user_email}}</text>
			<text class="cell-more yticon icon-you"></text>
		</view>
		<view class="list-cell" @click="navTo('../../set/grzl/nc/nc?ty=user_qq&name=QQ&v='+listgrzl.birthday)" hover-class="cell-hover" :hover-stay-time="50">
			<text class="cell-tit">QQ</text>
			<text class="cell-tip">{{userinfo.user_qq}}</text>
			<text class="cell-more yticon icon-you"></text>
		</view>
		<view class="list-cell">
			<text class="cell-tit">用户名</text>
			<text class="cell-tip">{{userinfo.user_name}}</text>
		</view>
		<view class="list-cell">
			<text class="cell-tit">手机号</text>
			<text class="cell-tip">{{userinfo.user_phone}}</text>
		</view>
<!-- 		
		<view class="list-cell b-b" @click="navTo('../../set/grzl/nc/nc?ty=sg&name=身高&v='+listgrzl.sg)">
			<text class="cell-tit">身份证号</text>
			<text class="cell-tip">{{listgrzl.sg}}</text>
			<text class="cell-more yticon icon-you"></text>
		</view> -->
		<avatar @upload="doUpload" @avtinit="doBefore" quality="1" ref="avatar"></avatar>
	</view>
</template>

<script>
	import avatar from "../../../components/yq-avatar/yq-avatar.vue";
	let sex = ['女', '男'];
	import { mapMutations} from 'vuex';
	export default {
		data() {
			return {
				avatar:'',
				sexsi:'',
				array: [],
				ageid: 0, 
				listgrzl:[],
				msg:false,
				user_name:'',
				userinfo:[],
				imgUrl:this.configs.Url
			};
		},
		computed: {
			sexType() {
				return sex[this.sexsi];
			},
		},
		onShow(){
			this.ongrzlTap();
			if(this.msg){
				uni.showToast({ title: '保存成功',icon:"none" });
				this.msg=false;
			}
		},
		methods:{
			...mapMutations(['logout']),
			...mapMutations(['login']),
			//跳转
			async ongrzlTap(){
				let data = {};
				data.user_random = uni.getStorageSync("userinfo").user_random;
				uni.request({
					url: this.configs.Url+'api.php/User/index',
					data: data,
					success: res =>{
						if(res.data.code==1){
							let ionfo=res.data.data
							ionfo.isLogin=true
							this.userinfo=ionfo
							this.vipLevel=ionfo.group_id
							this.user_name=ionfo.user_nick_name?ionfo.user_nick_name:ionfo.user_name
							if(this.vipLevel==3){
								this.vipname='VIP会员'
							}else{
								this.vipname='普通会员'
							}
							if(res.data.data.user_portrait){
								var str = res.data.data.user_portrait;
								if(str.indexOf("data:image") != -1){
									var avatar='';
								}else{
									if(str.indexOf("http") != -1){
										avatar=str;
									}else{
										avatar=this.configs.Url+str;
									}
								}
							}else{
								var avatar='';
							}
							this.avatar=avatar
						}else{
							uni.showToast({ title: res.data.msg,icon:"none" });
						}
					},
					fail: (data, code) => {
						//console.log('fail' + JSON.stringify(data));
					}
				});		
			},
			navTo(url){
				uni.navigateTo({
					url:url,
					animationType: 'slide-in-bottom',
				})
			},
			navsTo(url){
				uni.navigateTo({
					url:url
				})
			},
			//退出登录
			toLogout(){
				uni.showModal({
				    content: '确定要退出登录么',
				    success: (e)=>{
				    	if(e.confirm){
				    		uni.clearStorageSync();
				    		setTimeout(()=>{
								uni.navigateBack();
				    		}, 200)
				    	}
				    }
				});
			},
			//选择性别
			sex() {
				uni.showActionSheet({
					itemList:sex,
					success: (res)=> {
						this.sexgender(res);
					}
				});
			},
			async sexgender(ress) {
				let _this=this;
				let [err,res] = await this.$httpas.post('/api/user/edituser',{sex:ress.tapIndex},{token:true});
				if (!this.$httpas.errorCheck(err,res)) return;
				if(res.data.code === 1){
					  this.sexsi = ress.tapIndex,
					  uni.showToast({
						title:"保存成功"
					  })
				}else{
					uni.showToast({title:res.data.msg})
					this.logining = false;
				}
			},
			
			doBefore() {
				console.log('doBefore');
			},
			clk(index) {
				this.$refs.avatar.fChooseImg(index,{
					selWidth: '350upx', selHeight: '350upx', 
					expWidth: '400upx', expHeight: '400upx',
					inner: index ? 'true' : 'false',
					canRotate:false,
					quality:1,
				});
			},
			async doUpload(rsp) {
				uni.showLoading({
				    title: '加载中'
				});
				// let data = {};
				// data.filePath = rsp.path;
				// let [err,res] =await this.$httpas.upload('api.php/User/portrait',data,{token:true});
				// if (!this.$httpas.errorCheck(err,res)) return;
				console.log(rsp)
				
				uni.uploadFile({
					url: this.configs.Url+'api.php/User/portrait', //仅为示例，非真实的接口地址
					filePath: rsp.path,
					name: "file",
					header:{
						// 'Content-Type':'application/json;charset=UTF-8',
						// 'Content-Type':'application/x-www-form-urlencoded',
						"token": uni.getStorageSync("userinfo").user_random
					},
					dataType:"json",
					formData: {
						'user_random': uni.getStorageSync("userinfo").user_random,
						// #ifdef H5
						'base64':rsp.base64, //h5上传base64
						// #endif
					},
					success: (uploadFileRes) => {
						var dataarr=JSON.parse(uploadFileRes.data);
						if(dataarr.code==1){
							console.log(dataarr.data.file)
							this.avatar=this.configs.Url+dataarr.data.file;
							uni.setStorage({//缓存配置信息
								key: 'avatar',  
								data: this.avatar
							})
							uni.showToast({
								title:"修改成功"
							})
						}else{
							uni.showToast({
								title:dataarr.msg
							})
						}
					},
					complete(res) {
						uni.hideLoading();
						console.log(res)
					}
				});
			}
		},
		components: {
			avatar
		}
	}
</script>

<style lang='scss'>
	page{
		background: $page-color-base;
	}
	.yyt-ellipsis{
		display: inline-block;
		overflow:hidden; //超出的文本隐藏
		text-overflow:ellipsis; //溢出用省略号显示
		white-space:nowrap; //溢出不换
	}
	.avatar{
		padding-top: 40upx!important;
		padding-bottom: 40upx!important;
	}
	.agess{
		position: absolute;
		z-index: 2;
		top: 0px;
		right: 0px;
		height: 100%;
		line-height: 38px;
		padding-left: 200px;
		padding-right: 36px!important;
		background: transparent;
	}
	.avatar .cell-tit{
		font-weight: bold!important;
	}
	.avatar image{
		position: absolute;
		top: 50%;
		right: 36px;
		width: 88upx;
		height: 88upx;
		border-radius: 50%;
		margin-top: -44upx;
		z-index: 1
	}
	.list-cell{
		display:flex;
		align-items:baseline;
		padding: 20upx $page-row-spacing;
		line-height:60upx;
		position:relative;
		background: #fff;
		justify-content: center;
		&.log-out-btn{
			margin-top: 40upx;
			.cell-tit{
				color: $uni-color-primary;
				text-align: center;
				margin-right: 0;
			}
		}
		&.cell-hover{
			background:#fafafa;
		}
		&.b-b:after{
			left: 30upx;
		}
		&.m-t{
			margin-top: 16upx; 
		}
		.cell-more{
			align-self: baseline;
			font-size:$font-lg;
			color:$font-color-light;
			margin-left:10upx;
		}
		.cell-tit{
			flex: 1;
			font-size: $font-base + 2upx;
			color: $font-color-dark;
			margin-right:10upx;
		}
		.cell-tip{
			font-size: $font-base;
			color: $font-color-light;
		}
		switch{
			transform: translateX(16upx) scale(.84);
		}
	}
</style>
