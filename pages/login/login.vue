<template>
	<view class="container">
		<view>
			<label ><text space="emsp">身 份</text></label>
			<switch @change="switchChange" color="#07c160">{{status}}</switch ><text></text>
		</view>
		<view>
			<label for="username"><text>用户名</text></label>
			<input type="text" @input="getUserName" id="username" placeholder="请输入用户名" />
		</view>
		<view>
			<label for="password"><text space="emsp">密 码</text></label>
			<input type="text" @input="getPassword" password id="password" placeholder="请输入密码" />
		</view>
		<view>
			<button @click="login">登录</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				status:"学生",
				username : "",
				password : "",
				loginUrl : "https://api.bailiban.com/api/auth/jwt/token"
			}
		},
		methods: {
			switchChange(e){
				if(e.target.value){
					this.status = "老师"
				}else{
					this.status = "学生"
					this.loginUrl = "https://api.bailiban.com/api/auth/jwt/token"
				}
			},
			getUserName(event){
				this.username = event.target.value;
			},
			getPassword(event){
				this.password = event.target.value;
			},
			login(){
				uni.request({
					url:this.loginUrl,
					data:{
						username : this.username,
						password : this.password,
						type : 1
					},
					method:"POST",
					dataType:"json",
					success:(res)=> {
						console.log(res.data)
						if(res.data.status == 200){
							uni.setStorageSync("blbUI-token",res.data.data);
							uni.switchTab({
								url:"../user/user"
							})
						}else{
							uni.showModal({
								content:res.data.message,
								showCancel:false,
							});
						}
					}
				});
			}
			
		}
	}
</script>

<style lang="scss">
	
	.container{
		margin-top: 200rpx;
		text-align: center;
		view{
			width: 700rpx;
			margin: 0 auto;
			border-bottom: 1px solid #eee;
			display: flex;
			line-height: 100rpx;
			label{
				text-align: left;
			}
		   input{
				 margin: auto auto;
			}
			switch{
				margin: auto auto;
				color: #8080a1;
			}
			button{
				width: 100%;
				background-color: #07c160;
				margin-top: 20rpx;
			}
		}
	}

</style>
