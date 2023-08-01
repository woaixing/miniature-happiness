<template>
	<view>
		<view class="czmain">
			<view class="cztop">
				<view class="czyebox">
					<view style="font-size: 36upx; text-align: center;">当前余额 ￥{{urerdata.user_points}}</view>
					<view>提现金额</view>
					<input type="digit" focus class="czye" v-model="cash_money" placeholder="请输入提现金额" />
					<view>开户行</view>
					<input type="text" class="czye1" v-model="cash_bank_name" placeholder="请输入开户行名称或支付宝微信" />
					<view>提现姓名</view>
					<input type="text" class="czye1" v-model="cash_payee_name" placeholder="请输入收款人姓名与上方账户对应" />
					<view>提现账号</view>
					<input type="text" class="czye1" v-model="cash_bank_no" placeholder="请输入银行卡号或支付宝微信账号" />
					<view style="margin-top: 10upx; font-weight: 300; font-size: 24upx; color: #ff0000;">备注：
					1元等于{{urerdata.cash_ratio}}积分，最低提现金额：{{urerdata.cash_min}}元，
					剩余{{urerdata.user_points}}积分，相当于{{urerdata.user_points/urerdata.cash_ratio}}元；冻结{{urerdata.user_points_froze}}积分，相当于{{urerdata.user_points_froze/urerdata.cash_ratio}}元；
					</view>
					<view class="uni-btn-v uni-common-mt">
						<button  class="onstep" @click="butsub()">提交</button>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
    export default {
        data() {
            return {
                loading: false,
				
                cash_money: '',
				cash_payee_name: '',
				cash_bank_no:'',
				cash_bank_name:'',
				
				dbled:true,
				urerdata:[],
				configsa:[],
                providerList: []
            }
        },
        onLoad: function() {
			
        },
		onShow: function() {
			this.ongrzlTap()
		},
        methods: {
			async butsub(){
				var thia=this
				let data = {
					'token':uni.getStorageSync("userinfo").token,
					'cash_money':this.cash_money,//金额
					'cash_bank_no':this.cash_bank_no,//银行卡号或支付宝微信账号
					'cash_payee_name':this.cash_payee_name,//收款人姓名与上方账户对应
					'cash_bank_name':this.cash_bank_name,//开户行名称或支付宝微信
					};
				let [err,res] =await this.$httpas.post('api.php/User/cash',data,{token:true});
				if (!this.$httpas.errorCheck(err,res)) return;
				if(res.data.code == 1){
					uni.showModal({
						title: '温馨提示',
						content: '提现成功',
						showCancel: false,
						success: ress => {
							thia.cash_money=''
							thia.cash_bank_no=''
							thia.cash_bank_name=''
							thia.cash_payee_name=''
							thia.ongrzlTap()
						}
					});
				}else{
					uni.showModal({
						title: '温馨提示',
						content: res.data.msg,
						showCancel: false,
						success: ress => {
							thia.ongrzlTap()
						}
					});
				}
				
			},
			async ongrzlTap(){
				let data = {};
				data.user_random = uni.getStorageSync("userinfo").user_random;
				uni.request({
					url: this.configs.Url+'api.php/User/index',
					data: data,
					success: res =>{
						if(res.data.code==1){
							this.urerdata=res.data.data
						}else{
							uni.showToast({ title: res.data.msg,icon:"none" });
						}
					},
					fail: (data, code) => {
						//console.log('fail' + JSON.stringify(data));
					}
				});		
			},
			
			// 监听原生标题导航按钮点击事件
			onNavigationBarButtonTap(e) {
				const index = e.index;
					uni.navigateTo({
						url:"/pages/client/member/txjl"
					})
					
			}
        }
    }
</script>

<style>
	page{
		background: #f9f9f9;
	}
	.cztop{
		position: relative;
	}
	.czxx{
		top: 10px;
		font-size: 32upx;
		margin-left: 50upx;
		color: #3a3a3a;
		font-family: iconfont;
		line-height: 80upx;
	}
	.czyebox{
		padding: 20upx 50upx;
	}
	.czyebox>view:first-child{
		font-size: 32upx;
		color: #3a3a3a;
		line-height: 50px;
	}
	.czye{
		width: 100%;
		border-bottom: 1upx solid #e5e5e5;
		font-size: 36upx;
		font-weight: bold;
		color: #3a3a3a;
		position: relative;
		padding-left: 20px;
		height: 40px;
		line-height: 40px;
	}
	.czye:after{
		position: absolute;
		top: -2px;
		left: 0;
		content: '￥';
		color: #3a3a3a;
		font-size: 18px;
	}
	.czye1{
		width: 100%;
		border-bottom: 1upx solid #e5e5e5;
		font-size: 32upx;
		color: #3a3a3a;
		position: relative;
		height: 40px;
		line-height: 40px;
	}
	.onstep{
		margin-top: 40upx;
		background: #007AFF!important;
	}
	.czmain{
		width: 94%;
		margin: 10px auto;
		overflow: hidden;
		background: #fff;
		border-radius: 16upx;
		padding-bottom: 20px;
		-webkit-box-shadow: 0 0 20px rgba(0, 0, 0, 0.08);
		box-shadow: 0 0 20px rgba(0, 0, 0, 0.08);
	}
	.icon-alipay{
		background: #fbfbfb;
		padding-bottom: 20upx;
	}
	.icon-alipay>view:first-child{
		margin-top: 10px;
		font-size: 32upx;
		padding-left: 0upx;
		color: #3a3a3a;
		font-family: iconfont;
		position: relative;
		line-height: 30px;
	}
	.icon-alipay:before {
	    content: "\e636";
		margin-top: 25upx;
		margin-right: 10upx;
		color: #007AFF;
	}
	.cztype>view:first-child:after{
		position: absolute;
		content:"\e66c";
		top: 0;
		left: 0;
		font-size: 26px;
		color: #05c160;
	}


 .rmbLogo {
        font-size: 40upx;
    }

    button {
        background-color: #007aff;
        color: #ffffff;
    }

    .uni-h1.uni-center {
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: flex-end;
    }

    .ipaPayBtn {
        margin-top: 30upx;
    }
</style>
