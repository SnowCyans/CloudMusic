<template>
	<div>
		<text @click="logOut()" class="logOut cuIcon-my fs-36"></text>
		<div class="group">
			<div class="item item1"></div>
			<div class="item item2"></div>
			<div class="item item3"></div>
			<div class="item item4"></div>
		</div>
		
		
	</div>
</template>

<script>
	import gsap from 'gsap'
	export default {
		data() {
			return {
			};
		},
		mounted() {
			const items = document.querySelectorAll('.item')
			const expand = (item, i) => {
				items.forEach((it, ind) => {
					if (i === ind) return
					it.clicked = false
				})
				gsap.to(items, {
					width: item.clicked ? '15vw' : '8vw',
					duration: 2,
					ease: 'elastic(1, .6)',
				})

				item.clicked = !item.clicked
				gsap.to(item, {
					width: item.clicked ? '42vw' : '15vw',
					duration: 2.5,
					ease: 'elastic(1, .3)',
				})
			}

			items.forEach((item, i) => {
				item.clicked = false
				item.addEventListener('click', () => expand(item, i))
			})
		},
		methods:{
			// 退出登录
			logOut(){
				uni.navigateTo({
					url:'/pages/index/index'
				})
			}
		}
		
	}
</script>

<style lang="scss">
	page {
		height: 100vh;
		display: flex;
		margin: 0;
		background: linear-gradient(135deg, #d16ba5, #86a8e7, #5ffbf1);
		background-size: cover;
		align-items: center;
		justify-content: center;
		user-select: none;
	}

	.logOut {
		position: absolute;
		top: 20px;
		color:black;
	}
	.logOut:hover{
		transform: scale(1.2);
		color: #00c8b6;
		cursor: pointer;
	}
	@media only screen and (max-width:1540px) {
		.logOut {
			right: 140px;
		}
	}
	@media only screen and (max-width: 900px) {
		.logOut {
			right: 140px;
		}
	}
	.group {
		text-align: center;
		white-space: nowrap;
		overflow: hidden;
	}

	.item {
		width: 15vw;
		height: 75vh;
		background-position: center;
		background-size: 75vh;
		margin: 1vw;
		border-radius: 3vw;
		display: inline-block;
		cursor: pointer;
	}

	.item1 {
		background-image: url('../../static/image/AI1.jpg');
	}

	.item2 {
		background-image: url('../../static/image/AI6.png');
		background-position: 50% 40%;
	}

	.item3 {
		background-image: url('../../static/image/AI7.png');
	}

	.item4 {
		background-image: url('../../static/image/AI2.png');
		background-position: 50% 20%;
	}
	
	
</style>
