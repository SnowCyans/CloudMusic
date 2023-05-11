<template>
	<div class="login-box">
		<h5 class="color-lvse text-center fs-28">照片</h5>
		<h4>登录后查看</h4>
		<form action="">
			<div class="user-box">
				<input type="text" v-model="loginForm.account">
				<label>账号</label>
			</div>
			<div class="user-box">
				<input type="password" v-model="loginForm.password">
				<label>密码</label>
			</div>
			<text @click="handleCancel()">
				<a href="#">
					<span></span>
					<span></span>
					<span></span>
					<span></span>
					取消
				</a>
			</text>
			<text @click="handleLogin()">
				<a href="#" class="btn">
					<span></span>
					<span></span>
					<span></span>
					<span></span>
					确定
				</a>
			</text>

		</form>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				// 账号密码
				loginForm: {
					account: '123456789',
					password: '1234567890'
				}
			};
		},
		methods: {
			// 取消
			handleCancel() {
				uni.navigateTo({
					url: '/pages/index/index'
				})
			},
			// 登录
			handleLogin() {
				let {account,password} = this.loginForm
				if(account !== '123456789'){
					uni.showToast({
						icon: 'none',
						title: '账号不存在'
					})

				} else if(password !== '1234567890'){
					uni.showToast({
						icon:'none',
						title:'密码错误'
					})
				} else{
					uni.navigateTo({
						url: '/pages/girlShow/girlShow'
					})
				}
				
			}
		}
	}
</script>

<style lang="scss">
	page {
		margin: 0;
		padding: 0;
		font-family: sans-serif;
		background: linear-gradient(#141e30, #243b55);
	}

	@media only screen and (max-width:1540px) {
		.login-box h4 {
			font-size: 20px;
		}
	}

	.login-box {
		position: absolute;
		top: 50%;
		left: 50%;
		width: 400px;
		padding: 40px;
		transform: translate(-50%, -50%);
		background: rgba(0, 0, 0, .5);
		box-sizing: border-box;
		box-shadow: 0 15px 25px rgba(0, 0, 0, .6);
		border-radius: 10px;
	}

	.login-box h4 {
		margin: 0 0 30px;
		padding: 0;
		color: #bababa;
		text-align: center;
	}

	.login-box .user-box {
		position: relative;
	}

	.login-box .user-box input {
		width: 80%;
		height: 50px;
		padding: 10px 0;
		font-size: 16px;
		color: #00c8b6;
		margin-bottom: 50px;
		margin-left: 50px;
		border: none;
		border-bottom: 1px solid #fff;
		outline: none;
		background: transparent;
	}

	.login-box .user-box label {
		position: absolute;
		top: 20px;
		left: 0;
		padding: 10px 0;
		font-size: 16px;
		color: #fff;
		pointer-events: none;
		transition: .5s;
	}

	.login-box .user-box input:focus~label,
	.login-box .user-box input:valid~label {
		top: -20px;
		left: 0;
		color: #03e9f4;
		font-size: 12px;
	}

	.login-box form a {
		position: relative;
		display: inline-block;
		padding: 10px 20px;
		color: #03e9f4;
		font-size: 16px;
		text-decoration: none;
		text-transform: uppercase;
		overflow: hidden;
		transition: .5s;
		margin-top: 40px;
		letter-spacing: 4px
	}

	.login-box form .btn {
		margin-left: 150px;
	}

	.login-box a:hover {
		background: #03e9f4;
		color: #141e30;
		border-radius: 5px;
		box-shadow: 0 0 5px #03e9f4,
			0 0 25px #03e9f4,
			0 0 50px #03e9f4,
			0 0 100px #03e9f4;
	}

	.login-box a span {
		position: absolute;
		display: block;
	}

	.login-box a span:nth-child(1) {
		top: 0;
		left: -100%;
		width: 100%;
		height: 2px;
		background: linear-gradient(90deg, transparent, #03e9f4);
		animation: btn-anim1 1s linear infinite;
	}

	.login-box form .btn span:nth-child(1) {
		animation: btn-anim1to 1s linear infinite;

	}

	@keyframes btn-anim1 {
		0% {
			left: -100%;
		}

		50%,
		100% {
			left: 100%;
		}
	}

	@keyframes btn-anim1to {
		0% {
			left: 100%;
		}

		50%,
		100% {
			left: -100%;
		}
	}

	.login-box a span:nth-child(2) {
		top: -100%;
		right: 0;
		width: 2px;
		height: 100%;
		background: linear-gradient(180deg, transparent, #03e9f4);
		animation: btn-anim2 1s linear infinite;
		animation-delay: .25s
	}

	.login-box form .btn span:nth-child(2) {
		animation: btn-anim2to 1s linear infinite;
		animation-delay: .75s
	}

	@keyframes btn-anim2 {
		0% {
			top: -100%;
		}

		50%,
		100% {
			top: 100%;
		}
	}

	@keyframes btn-anim2to {
		0% {
			top: 100%;
		}

		50%,
		100% {
			top: -100%;
		}
	}

	.login-box a span:nth-child(3) {
		bottom: 0;
		right: -100%;
		width: 100%;
		height: 2px;
		background: linear-gradient(270deg, transparent, #03e9f4);
		animation: btn-anim3 1s linear infinite;
		animation-delay: .5s
	}

	.login-box form .btn span:nth-child(3) {
		animation: btn-anim3to 1s linear infinite;
		animation-delay: .5s
	}

	@keyframes btn-anim3 {
		0% {
			right: -100%;
		}

		50%,
		100% {
			right: 100%;
		}
	}

	@keyframes btn-anim3to {
		0% {
			right: 100%;
		}

		50%,
		100% {
			right: -100%;
		}
	}

	.login-box a span:nth-child(4) {
		bottom: -100%;
		left: 0;
		width: 2px;
		height: 100%;
		background: linear-gradient(360deg, transparent, #03e9f4);
		animation: btn-anim4 1s linear infinite;
		animation-delay: .75s
	}

	.login-box form .btn span:nth-child(4) {
		animation: btn-anim4to 1s linear infinite;
		animation-delay: .25s
	}

	@keyframes btn-anim4 {
		0% {
			bottom: -100%;
		}

		50%,
		100% {
			bottom: 100%;
		}
	}

	@keyframes btn-anim4to {
		0% {
			bottom: 100%;
		}

		50%,
		100% {
			bottom: -100%;
		}
	}
</style>
