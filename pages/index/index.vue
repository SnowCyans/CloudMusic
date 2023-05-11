<template>
	<div class="container">
		<div class="left-music">
			<div class="recommend flex justify-between align-center ">
				<div class="fs-18">推荐</div>
				<text @click="aiGirl()" class=" scales pointer cuIcon-friendfavor color-lvse"></text>
			</div>
			<div class="lists-like">
				<div @click="category(1)" class="pointer song-play-list lists"
					:class="[likeListShow ? 'color-huise2' : 'color-lvse']">列表</div>
				<div @click="category(2)" class="pointer song-play-list like"
					:class="[likeListShow ? 'color-lvse' : 'color-huise2']">喜欢</div>
			</div>
			<!-- 推荐歌曲 -->
			<div>
				<image src="../../static/image/city.jpg" class="recommend-img recommend-img1" mode="aspectFill"></image>
				<image @click="recommendSong(recommend[0])" src="../../static/image/hg.jpg"
					class="recommend-img recommend-img2" mode="aspectFill"></image>
				<image src="../../static/image/city.jpg" class="recommend-img recommend-img3" mode="aspectFill"></image>
				<div class="re-songDetail">
					<div>
						<text>{{recommend[0].song}}</text>-
						<text>{{recommend[0].singer}}</text>
					</div>
					<text class="pointer  fs-26" :class="[stopFlag ? 'cuIcon-stop color-lvse' : 'cuIcon-video']"></text>
				</div>

			</div>
			<!-- 列表歌曲 -->
			<div v-if="likeListShow === false">
				<div v-for="(item,i) in defaultSongList" :key="i" class="songlist">
					<div @click="handleSong(item)" class="pointer song flex align-center justify-between padding-right">
						<div class="flex align-center">
							<image :src="item.img" class="imagelist" mode="aspectFill"></image>
							<text class="songlist-audio  fs-22"
								:class="[item.index === playIndex ? 'cuIcon-stop color-lvse' : 'cuIcon-video']"></text>
							<div>
								<div class="song-name">{{item.song}}</div>
								<div class="singer-name">{{item.singer}}</div>
							</div>
						</div>
						<!-- MV图标 -->
						<text @click.stop="liVideo(item)" class="liVideo"
							:class="[item.index === videoIndex ? 'cuIcon-recordfill' : 'cuIcon-record']"></text>
						<text @click.stop="songListLike(item,i)" class="liLike"
							:class="[item.flag ? 'cuIcon-likefill' : 'cuIcon-like']"></text>
					</div>
				</div>
			</div>
			<!-- 喜欢歌曲 -->
			<div v-else>
				<div v-for="(item,i) in likeSongList" :key="i" class="songlist">
					<div @click="handleSong(item)" class="pointer song flex align-center justify-between padding-right">
						<div class="flex align-center">
							<image :src="item.img" class="imagelist" mode="aspectFill"></image>
							<text class="songlist-audio  fs-18"
								:class="[item.index === playIndex ? 'cuIcon-stop color-lvse' : 'cuIcon-video']"></text>
							<div>
								<div class="song-name">{{item.song}}</div>
								<div class="singer-name">{{item.singer}}</div>
							</div>
						</div>
						<!-- MV图标 -->
						<text @click.stop="liVideo(item)" class="liVideo "
							:class="[item.index === videoIndex ? 'cuIcon-recordfill' : 'cuIcon-record']"></text>
						<text @click.stop="songListLike(item,i)" class="liLike"
							:class="[item.flag ? 'cuIcon-likefill' : 'cuIcon-like']"></text>
					</div>
				</div>
				<!-- 喜欢歌曲为空 -->
				<div v-if="likeSongList.length === 0" class="likeEmpty"><text>还没有添加喜欢歌曲哦！</text></div>
			</div>
			<!-- 左侧底部歌曲 -->
			<div class="playlist">
				<div class="left-song flex align-center justify-between padding-sm">
					<image @click="previewImg()" :src="audioList.img.toString()" class="left-song-img" :class="[active ? 'roteActive' : '']" mode="aspectFill">
					</image>
					<div>
						<div class="song-name">{{audioList.song}}</div>
						<div class="singer-name">{{audioList.singer}}</div>
					</div>
					<text @click="songListLike(audioList,audioList.index)" class="left-bs-like"
						:class="[audioList.flag ? 'cuIcon-likefill' : 'cuIcon-like']"></text>
					<div class="left-b-play">
						<text @click="handleSongPlay()" class="hover-video2 pointer  fs-28"
							:class="[active ? 'cuIcon-stop color-lvse' : 'cuIcon-video color-huise2']"></text>
					</div>
				</div>
			</div>
			<!-- tabbar -->
			<div class="tabbar">
				<div v-if="searchShow === false"
					class="flex justify-between align-center padding-sm color-huise2 fs-18">
					<text @click="searchShow = true" class="pointer cuIcon-search"></text>
					<text @click="tbList()" class="pointer cuIcon-sort"></text>
					<text @click="likeListShow = !likeListShow" class="pointer cuIcon-like color-red"></text>
				</div>
				<div v-else class="flex align-center justify-between color-huise2 fs-22">
					<text @click="searchShow = false" class="pointer cuIcon-back margin-right-xs"></text>
					<input class="search-text" type="text" value="">
					<text class="pointer cuIcon-search  padding-left-xs"></text>
				</div>
			</div>
		</div>
		<!-- 右侧播放器 -->
		<div class="right-music">
			<!-- 头部 -->
			<div class="right-top flex align-center justify-between padding fs-20 color-fff"
				:class="[videoShow ? 'right-top-bg' : '']">
				<text @click="videoPlayPause(audioList)" class="scales pointer "
					:class="[videoShow ? 'cuIcon-musicfill' : 'cuIcon-record']"></text>
				<text class="fs-16">{{videoShow ? 'MV' : '音乐'}}</text>
				<a href="https://github.com/SnowCyans?tab=repositories" class="scales cuIcon-github color-fff"></a>
			</div>
			<div>
				<div :class="[active ? 'effect-music' : '']">
					<div class="wave"></div>
					<div class="wave"></div>
					<div class="wave"></div>
					<div class="wave"></div>
				</div>
				<image @click="previewImg()" :src="audioList.img.toString()" class="playActive"
					:class="[active ? 'roteActive' : '']" mode="aspectFill"></image>
				<!-- 音乐效果 -->
				<div class="box">
					<ul>
						<p v-for="item in 40" :class="active ? 'boxAnimation' : ''"></p>
					</ul>
				</div>
				<!-- 歌曲信息 -->
				<div class="song-detail">
					<div class="">{{audioList.song}}</div>
					<div class="fs-16 color-lvse">{{audioList.singer}}</div>
				</div>
				<!-- 时间 -->
				<div class="duration">
					<text>0:39</text>
					<text class="cuIcon-lightfill color-lvse"></text>
					<text>3:20</text>
				</div>
				<!-- 操作播放歌曲区 -->
				<div class="operation flex justify-between align-center padding-xs padding-tb-sm fs-22">
					<text @click="videoPlayPause(audioList)"
						class="scales cuIcon-record margin-left-xs color-fff"></text>
					<text @click="handlefront(audioList.index)" class="scales cuIcon-back color-huise3"></text>
					<div @click="handlePlay(audioList.index)" class="scales color-huise3">
						<text class="fs-36" :class="[active ? 'cuIcon-stop color-lvse ' : 'cuIcon-video']"></text>
					</div>
					<text @click="handleafter(audioList.index)" class="scales cuIcon-right color-huise3"></text>
					<text @click="rpLike(audioList)" class="likeScale pointer color-red"
						:class="[audioList.flag ? 'cuIcon-likefill' : 'cuIcon-like']"></text>
				</div>
			</div>
			<!-- mv -->
			<video v-if="videoShow" class="videos" :src="audioList.video" autoplay controls
				webkit-playsinline="true"></video>
		</div>

	</div>
</template>

<script>
	const innerAudioContext = uni.createInnerAudioContext() //把该对象变为全局属性
	export default {
		data() {
			return {
				// 推荐歌曲数据
				recomList: [],
				// 判断显示列表还是喜欢歌曲
				likeListShow: false,
				// 是否喜欢歌曲
				likeShow: false,
				// 喜欢歌曲索引
				likeIndex: -1,
				// 是否显示搜素框
				searchShow: false,
				// 列表歌曲数据
				songList: [{
						song: '芒种伴奏',
						singer: '伴奏',
						img: '../../static/image/AI1.jpg',
						flag: false,
						audioUrl: '../../static/music/mangzhong.mp3',
						index: 0,
						video: '../../static/video/mz.avi'
					},
					{
						song: '启强落幕曲',
						singer: '王贰浪',
						img: '../../static/image/g.jpg',
						flag: false,
						audioUrl: '../../static/music/qqlmq.mp3',
						index: 1,
						video: '../../static/video/qqlmq.avi'
					},
					{
						song: '心墙',
						singer: '世界杯',
						img: '../../static/image/nmr.jpg',
						flag: false,
						audioUrl: '../../static/music/xqDj.mp3',
						index: 2,
						video: '../../static/video/xqDj.avi'
					},
					{
						song: '指纹',
						singer: '胡歌',
						img: '../../static/image/hg.jpg',
						flag: false,
						audioUrl: '../../static/music/zhiwen.mp3',
						index: 3,
						video: '../../static/video/zhiwen.avi'
					},
					{
						song: '答案',
						singer: 'DJ',
						img: '../../static/image/city.jpg',
						flag: true,
						audioUrl: '../../static/music/hkslDj.mp3',
						index: 4,
						video: '../../static/video/hksl.avi'
					}
				],
				// 判断是否开启旋转动画
				active: false,
				// 当前播放音乐数据
				audioList: {},
				// 是否暂停歌曲
				playStopFlag: false,
				// 是否把图标改为暂停(不包含列表和喜欢区)
				stopFlag: false,
				// 是否把图标改为暂停 (列表和喜欢区)
				playIndex: -1,
				// 上一首歌曲
				frontFlag: true,
				// 下一首歌曲
				afterFlag: true,
				// 歌曲播放位置
				currentTime: null,
				// 是否显示mv
				videoShow: false,
				// 当前MV图标索引
				videoIndex: -1,
			}
		},
		onLoad() {
			this.songList.forEach((item) => {
				if (item.index === 2) {
					// 默认播放歌曲
					this.audioList = item
				}
			})
		},
		methods: {
			// 跳转至登录页面
			aiGirl() {
				uni.navigateTo({
					url: "/pages/login/login"
				})
			},
			// 点击推荐歌曲
			recommendSong(recommend) {
				this.stopFlag = true //推荐歌曲图标设为播放
				this.playIndex = -1 //关闭列表歌曲图标播放状态
				this.audioList = recommend
				if (this.playStopFlag) {
					this.qqSongPause()
				} else {
					this.qqSongPlay(recommend.audioUrl)
					this.playStopFlag = true
				}
			},
			// 切换列表和喜欢显示
			category(flag) {
				if (flag === 1) {
					this.likeListShow = false
				} else {
					this.likeListShow = true
				}
			},
			// 点击列表或喜欢里的歌曲进行播放
			handleSong(item) {
				// 当前播放歌曲图标是否打开状态
				if (this.playIndex === -1) {
					this.playIndex = item.index
					this.audioList = item
				} else {
					this.playIndex = -1
				}
				//如果MV界面打开中将MV图标状态改为播放
				if (this.videoShow) {
					this.videoIndex = item.index
				}
				this.playIndex = item.index
				this.audioList = item
				this.songPlays(item.audioUrl)
			},
			// 点击列表和喜欢区喜欢或取消喜欢歌曲
			songListLike(item, i) {
				if (item.flag) {
					this.songList[item.index].flag = false
					this.myToast('已取消')
				} else {
					this.songList[item.index].flag = true
					this.myToast('收藏成功')
				}
			},
			// 预览图片
			previewImg() {
				// 如果这个参数为String类型就将他转为数组
				if (typeof(this.audioList.img) === 'string') {
					this.audioList.img = this.audioList.img.split()
				}
				// 获取图片路径是一个数组
				uni.previewImage({
					current: 0,
					urls: this.audioList.img,
					longPressActions: {
						itemList: ['保存图片'],
						success: (res) => {
							console.log(res);
							let imgurl = this.audioList.img.toString()
							this.saveImage(imgurl)
						}
					}
				})
			},
			// 左侧底部播放按钮
			handleSongPlay() {
				if (this.playStopFlag) {
					this.qqSongPause()
				} else {
					this.qqSongPlay(this.audioList.audioUrl)
					this.playStopFlag = true
				}
			},
			// 左侧底部列表
			tbList() {
				if (this.likeListShow) {
					this.likeListShow = false
				} else {
					this.myToast('当前显示的是列表歌曲')
				}
			},
			// 关闭或右侧播放器打开MV
			videoPlayPause(item) {
				if (this.videoShow) {
					this.videoShow = false
					this.videoIndex = -1
				} else {
					this.videoShow = true
					this.videoIndex = item.index
					this.qqSongPause() //暂停歌曲
				}
			},
			// 播放当前歌曲MV
			liVideo(item) {
				this.audioList = item //把当前信息给播放列表
				if (this.videoIndex === -1 || this.playIndex === -1) {
					this.videoIndex = item.index //把当前索引给MV索引
					this.audioList = item //把MV数据给播放列表
					this.videoShow = true
					this.playIndex = -1 //歌曲播放图标关闭
					this.qqSongPause()
				} else {
					this.playIndex = -1
					this.videoIndex = -1
					this.videoShow = false
				}
				console.log(this.videoIndex);
			},
			// 上一首
			handlefront(index) {
				// 获取当前歌曲的上一首歌
				if (this.frontFlag) {
					this.afterFlag = true
					this.audioList = this.songList[index - 1]
					console.log('播放');
					this.songPlays(this.audioList.audioUrl)
					if (this.audioList.index === 0) {
						this.frontFlag = false
					}
				} else {
					this.myToast('前面没有了')
				}

			},
			// 右侧播放按钮
			handlePlay() {
				this.playIndex = -1
				if (this.playStopFlag) {
					this.qqSongPause()
				} else {
					this.qqSongPlay(this.audioList.audioUrl)
					this.playStopFlag = true
				}
			},
			// 下一首
			handleafter(index) {
				// 获取当前歌曲的上一首歌
				if (this.afterFlag) {
					this.frontFlag = true
					this.audioList = this.songList[index + 1]
					this.songPlays(this.audioList.audioUrl)
					if (this.audioList.index === 4) {
						this.afterFlag = false
					}
				} else {
					this.myToast('后面没有了')
				}
			},
			// 右侧播放器喜欢图标
			rpLike(item) {
				console.log(item);
				if (item.flag) {
					this.audioList.flag = false
					this.myToast('已取消')
				} else {
					this.audioList.flag = true
					this.myToast('收藏成功')
				}
			},
			// 封装列表喜欢和切换歌曲播放
			songPlays(audioUrl) {
				if (this.videoShow) {
					this.myToast('正在播放MV')
					this.playIndex = -1 //不开启列表歌曲播放图标
				} else {
					this.stopFlag = false
					this.playStopFlag = true // 开启播放
					this.active = true //开启旋转动画
					innerAudioContext.src = audioUrl
					innerAudioContext.loop = true //循环播放
					innerAudioContext.playbackRate = 2.0
					innerAudioContext.play() //播放歌曲
				}
			},
			// 封装歌曲播放
			qqSongPlay(audioUrl) {
				if (this.videoShow) {
					this.myToast('正在播放MV')
					this.stopFlag = false //推荐歌曲图标设为关闭
				} else {
					if (this.audioList.index === 4) {
						this.stopFlag = true
					}
					this.active = true //开启旋转动画
					innerAudioContext.src = audioUrl;
					innerAudioContext.seek(this.currentTime) //从暂停处开始播放
					innerAudioContext.loop = true //循环播放
					innerAudioContext.playbackRate = 1.5
					innerAudioContext.play() //开始播放音乐
				}
			},
			// 封装歌曲暂停
			qqSongPause() {
				this.active = false //关闭旋转动画
				this.stopFlag = false //图标设为关闭
				innerAudioContext.pause() //停止播放音乐
				let {
					currentTime,
					duration
				} = innerAudioContext
				// 当前播放位置
				this.currentTime = currentTime
				this.playStopFlag = false
			},
			// 封装提示消息
			myToast(title) {
				uni.showToast({
					icon: 'none',
					title: title
				})
			},
			// 封装获取歌曲总时长
			formatSecond(seconds) {
				let m = Math.floor((seconds / 60 % 60)) < 10 ? '0' + Math.floor((seconds / 60 % 60)) : Math.floor((
					seconds / 60 % 60));
				let s = Math.floor((seconds % 60)) < 10 ? '0' + Math.floor((seconds % 60)) : Math.floor((seconds % 60));
				console.log(m + ":" + s);
				return m + ":" + s;
			}

		},
		computed: {
			// 推荐歌曲
			recommend() {
				return this.songList.filter((item, i) => {
					return i === 3
				})
			},
			// 筛选列表歌曲
			defaultSongList() {
				return this.songList.filter((item, i) => {
					return i < 3
				})
			},
			// 筛选喜欢歌曲
			likeSongList() {
				return this.songList.filter((item) => {
					return item.flag
				})
			}
		}

	}
</script>

<style lang="scss">
	page {
		background: linear-gradient(135deg, #d16ba5, #86a8e7, #5ffbf1);
	}

	.container {
		background-image: url("../../static/image/city2.jpg");
		width: 800px;
		height: 650px;
		position: relative;
		top: 0px;
		left: 0px;
		margin: auto;
		overflow: hidden;
		border-radius: 6px;
		box-shadow: 1px 2px 29px 2px #dcf4fa;
	}

	.left-music {
		position: absolute;
		left: 24%;
		top: 50%;
		transform: translate(-50%, -50%);
		width: 275px;
		height: 542px;
		border-radius: 23px;
		overflow: hidden;
		box-shadow: -6px 14px 40px -20px rgba(0, 0, 0, 0.2);
		border-radius: 20px;
		border: 1px solid #3b4358;
	}

	// 推荐栏
	.recommend {
		padding: 15px;
		color: #fff;
		text {
			font-size: 24px;
		}
	}

	// 推荐歌曲通用样式
	.recommend-img {
		border-radius: 15px;
		box-shadow: -10px 10px 30px -20px rgba(0, 0, 0, 0.5);
		margin-left: 60px;
		cursor: pointer;
	}

	// 推荐第一首歌
	.recommend-img1 {
		position: absolute;
		top: 70px;
		left: -150px;
		width: 130px;
		height: 130px;
	}

	// 推荐第二首歌
	.recommend-img2 {
		width: 160px;
		height: 160px;
	}

	.recommend-img1:hover {
		transform: scale(1.02);
	}

	.recommend-img2:hover {
		transform: scale(1.02);
	}

	// 推荐第三首歌
	.recommend-img3 {
		position: absolute;
		top: 70px;
		right: -95px;
		width: 130px;
		height: 130px;
		border-radius: 15px;
		box-shadow: -10px 10px 30px -20px rgba(0, 0, 0, 0.5);
	}

	.recommend-img3:hover {
		transform: scale(1.02);
		cursor: pointer;
	}

	// 推荐歌曲信息
	.re-songDetail {
		position: absolute;
		top: 185px;
		left: 65px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		width: 150px;
		color: #242424;
		font-size: 13px;
		font-weight: 800;
	}

	// 列表和喜欢区域
	.lists-like {
		position: absolute;
		top: 150px;
		left: -15px;
	}

	.song-play-list {
		font-size: 14px;
		position: absolute;
		left: 2px;
		top: 40px;
	}

	// 左侧边列表
	.lists {
		left: 30px;
		top: 90px;
	}

	.lists:hover {
		color: #00c8b6 !important;
	}

	// 左侧边喜欢
	.like {
		left: 30px;
		top: 170px;
	}

	.like:hover {
		color: #00c8b6 !important;
	}

	// 喜欢为空情况下样式
	.likeEmpty {
		position: absolute;
		left: 52%;
		top: 60%;
		transform: translate(-50%, -50%);
		font-size: 12px;
		color: #00c8b6;
	}

	// 列表和喜欢歌曲区
	.songlist {
		margin-left: 50px;
		margin-top: 10px;
		position: relative;
		top: 0;
		left: 0;
	}

	// 图片
	.imagelist {
		width: 57px;
		height: 57px;
		border-radius: 15px;
		margin-right: 15px;
		margin-left: 5px;
	}

	// 歌曲名样式
	.song-name {
		font-size: 14px;
		color: #fff;
		margin-top: -5px;
	}

	// 歌手名样式
	.singer-name {
		font-size: 12px;
		color: #00c8b6;
	}

	// 列表和喜欢区歌曲图片上的图标
	.songlist-audio {
		position: absolute;
		left: 12%;
		top: 50%;
		transform: translate(-50%, -50%);
		color: #b6b6b6;
	}

	.song:hover {
		transform: scale(1.02);
		box-shadow: -8px 10px 3px -11px rgba(0, 0, 0, 0.4);
	}

	// 收藏图片
	.liLike {
		position: absolute;
		top: 5px;
		right: 28px;
		color: red;
		font-size: 18px;
	}

	.liLike:hover {
		transform: scale(1.2);
	}

	// MV图标
	.liVideo {
		position: absolute;
		bottom: 5px;
		right: 28px;
		color: #00c8b6;
		font-size: 20px;
	}

	.liVideo:hover {
		transform: scale(1.2);
	}

	// 鼠标移入将列表和喜欢区歌曲图片上的图标变色
	.song:hover .songlist-audio {
		color: #00c9b7;
	}

	// 左侧底部歌曲区
	.playlist {
		position: absolute;
		top: 438px;
		left: 0;
		height: 60px;
		width: 100%;
		border-radius: 25px 25px 0 0;
		display: flex;
	}
	
	// 左侧底部歌曲图片
	.left-song-img{
		width: 55px;
		height: 55px;
		border-radius: 50%;
		box-sizing: border-box;
		cursor: pointer;
		animation: rotateImg 6s linear 0s infinite forwards;
		animation-play-state: paused;
	}
	// 左侧底部歌曲
	.left-song {
		width: 100%;
		box-shadow: 16px 14px 40px -10px rgba(0, 0, 0, 0.2);
	}
	// 喜欢图标
	.left-bs-like{
		font-size: 20px;
		color: red;
		cursor: pointer;
	}
	.left-bs-like:hover{
		transform: scale(1.2);
	}

	// 左侧底部播放图标
	.left-b-play {
		width: 32px;
		height: 32px;
		border-radius: 50%;
		display: flex;
	}

	.left-b-play:hover .hover-video2 {
		color: #00c8b6 !important;
	}

	// 左侧tabbar
	.tabbar {
		position: absolute;
		bottom: 0;
		color: #c5c7da;
		left: 0;
		width: 100%;
		height: 52px;

		// 搜素框
		.search-text {
			color: #fff;
			border: 1px solid #fff;
			width: 100%;
			height: 28px;
			border-radius: 50px;
			background-color: #fff;
		}
	}


	// 右侧效果
	.right-music {
		position: absolute;
		left: 0;
		top: 0;
		transform: translate(-50%, -50%);
		width: 275px;
		height: 542px;
		box-shadow: -6px 14px 40px -20px rgba(0, 0, 0, 0.2);
		border-radius: 20px;
		border: 1px solid #3b4358;
	}

	// 右侧top
	.right-top-bg {
		background-color: #242424;
		border-radius: 20px 20px 0 0;
	}
	
	.right-top a{
	  text-decoration: none;
	  color: #fff;
	}

	// 右侧播放音效
	.effect-music {
		position: absolute;
		top: 40px;
		left: 35px;
		margin: auto;
		width: 200px;
		height: 200px;
		overflow: hidden;
	}

	.effect-music>.wave {
		position: absolute;
		opacity: 0;
		left: 0;
		right: 0;
		top: 0;
		bottom: 0;
		margin: auto;
		width: 95px;
		height: 95px;
		border-radius: 50%;
		border: 2px solid #eee;
		animation: wave 4s linear 0s infinite;
		-webkit-animation: wave 2s linear 0s infinite;
	}

	.effect-music>.wave::after {
		content: "";
		position: absolute;
		top: -4px;
		left: 50%;
		width: 6px;
		height: 6px;
		overflow: hidden;
		border-radius: 5px;
		-webkit-border-radius: 5px;
		-moz-border-radius: 5px;
		-ms-border-radius: 5px;
		-o-border-radius: 5px;
		background-color: #ccc;
	}

	.effect-music>.wave:nth-child(2) {
		animation-delay: 0.5s;
	}

	.effect-music>.wave:nth-child(3) {
		animation-delay: 1s;
	}

	.effect-music>.wave:nth-child(4) {
		animation-delay: 1.5s;
	}

	@keyframes wave {
		from {
			opacity: 1;
			transform: rotate(0deg) scale(1);
			-webkit-transform: rotate(0deg) scale(1);
			-moz-transform: rotate(0deg) scale(1);
			-ms-transform: rotate(0deg) scale(1);
			-o-transform: rotate(0deg) scale(1);
		}

		to {
			opacity: 0;
			transform: rotate(-300deg) scale(2.2);
			-webkit-transform: rotate(-300deg) scale(2.2);
			-moz-transform: rotate(-300deg) scale(2.2);
			-ms-transform: rotate(-300deg) scale(2.2);
			-o-transform: rotate(-300deg) scale(2.2);
		}
	}

	// 右侧歌曲图片
	.playActive {
		box-sizing: border-box;
		width: 160px;
		height: 160px;
		border-radius: 50%;
		border: 7px solid #fff;
		position: relative;
		top: 0px;
		left: 57px;
		cursor: pointer;
		animation: rotateImg 6s linear 0s infinite forwards;
		animation-play-state: paused;
	}

	// 右侧歌曲信息
	.song-detail {
		text-align: center;
		margin-top: 27px;
		color: #fff;
	}

	// 播放时间
	.duration {
		margin: 90px 40px 10px 108px;
		display: flex;
		align-items: center;
		font-size: 14px;
		font-weight: 500;
		color: #d1d2e6;
	}

	// 右侧播放栏
	.operation {
		color: #242424 !important;
	}

	// MV图标
	.videos {
		position: absolute;
		top: 50px;
		left: 0px;
		width: 274px;
		height: 493px;
		border-radius: 0 0 20px 20px;
	}

	// 播放按钮
	.playbutton {
		border-radius: 50%;
		width: 35px;
		height: 35px;
		line-height: 33px;
		text-align: center;
		cursor: pointer;

		text {
			margin-left: 2px;
		}
	}

	// 右侧操作栏收藏图标
	.likeScale:hover {
		transform: scale(1.3);
	}

	// 封装手势鼠标
	.pointer {
		cursor: pointer;
	}

	.scales:hover {
		transform: scale(1.2);
		color: #00c8b6 !important;
		cursor: pointer;
	}

	// 开启动画运行
	.roteActive {
		animation-play-state: running;
	}



	// 旋转动画
	@keyframes rotateImg {
		0% {
			transform: rotateZ(0);
		}

		100% {
			transform: rotateZ(360deg);
		}
	}

	// 屏幕适配
	// PC全屏
	@media only screen and (max-width:1540px) {
		.container {
			background-size: cover;
			width: 1200px;
			transform: scale(0.98);
			margin-top: 30px;
		}

		// 左播放器
		.left-music {
			left: 33%;
			top: 50%;
			transform: translate(-50%, -50%);
		}


		//右面播放器
		.right-music {
			left: 65%;
			top: 50%;
			transform: translate(-50%, -50%);
		}

		.left-song {
			padding: 20px;
		}

		// 左侧tabbar
		.tabbar {
			div {
				padding: 15px;
			}
		}

		.right-top {
			padding: 20px;
		}

		.operation {
			padding: 20px;
		}
	}

	// PC半屏
	@media only screen and (max-width: 900px) {
		.container {
			background-size: cover;
			width: 700px;
			margin-top: 30px;
		}

		// 左播放器
		.left-music {
			left: 21%;
			top: 50%;
			transform: translate(-50%, -50%);
		}

		//右面播放器
		.right-music {
			left: 78%;
			top: 50%;
			transform: translate(-50%, -50%);
		}

		.playActive {
			top: 0px;
		}

		.right-top {
			padding: 20px;
		}
	}

	// PC小屏
	@media only screen and (max-width: 500px) {
		.container {
			background-size: 100% 100%;
			width: 700px;
			height: 950px;
			transform: scale(0.6);
			top: -145px;
			left: -100px;
		}

	}

	// 手机屏幕大小
	@media only screen and (max-width: 400px) {
		.container {
			background-image: url('../../static/image/city.jpg');
			background-size: 100% 100%;
			width: 622px;
			height: 1000px;
			top: -229px;
			left: -123px;
		}

		.left-music {
			top: 270px;
			left: 15px;
			transform: scale(1.1);
		}

		.right-music {
			top: 270px;
			left: 328px;
			transform: scale(1.1);
		}

	}

	// 音效动画
	.box {
		position: absolute;
		top: 340px;
		left: 0px;
		width: 250px;
		height: 50px;
		align-items: center;
	}

	// 开启播放音效动画
	.boxAnimation {
		animation: Mymove2 1s ease infinite;
	}

	.box ul {
		width: 190px;
		height: 26px;
		display: flex;
		transform: rotateX(180deg);
		justify-content: space-between;
		animation-play-state: paused;
	}

	.box p {
		width: 4px;
		height: 20px;
		background: #b5b5b5;
		border-radius: 4px;
	}

	.box p:nth-of-type(1) {
		animation-delay: .3s;
	}

	.box p:nth-of-type(2) {
		animation-delay: .7s;
	}

	.box p:nth-of-type(3) {
		animation-delay: .5s;
	}

	.box p:nth-of-type(4) {
		animation-delay: .3s;
	}

	.box p:nth-of-type(5) {
		animation-delay: .7s;
	}

	.box p:nth-of-type(6) {
		animation-delay: 3s;
	}

	.box p:nth-of-type(7) {
		animation-delay: .5s;
	}

	.box p:nth-of-type(8) {
		animation-delay: .7s;
	}

	.box p:nth-of-type(9) {
		animation-delay: .3s;
	}

	.box p:nth-of-type(10) {
		animation-delay: .5s;
	}

	.box p:nth-of-type(11) {
		animation-delay: .7s;
	}

	.box p:nth-of-type(12) {
		animation-delay: .7s;
	}

	.box p:nth-of-type(13) {
		animation-delay: .3s;
	}

	.box p:nth-of-type(14) {
		animation-delay: .5s;
	}

	.box p:nth-of-type(15) {
		animation-delay: .7s;
	}

	.box p:nth-of-type(16) {
		animation-delay: .5s;
	}

	.box p:nth-of-type(17) {
		animation-delay: .3s;
	}

	.box p:nth-of-type(18) {
		animation-delay: .5s;
	}

	.box p:nth-of-type(19) {
		animation-delay: .7s;
	}

	.box p:nth-of-type(20) {
		animation-delay: .3s;
	}

	.box p:nth-of-type(21) {
		animation-delay: .3s;
	}

	.box p:nth-of-type(22) {
		animation-delay: .5s;
	}

	.box p:nth-of-type(23) {
		animation-delay: .7s;
	}

	.box p:nth-of-type(24) {
		animation-delay: .3s;
	}

	.box p:nth-of-type(25) {
		animation-delay: .5s;
	}

	.box p:nth-of-type(26) {
		animation-delay: .7s;
	}

	.box p:nth-of-type(27) {
		animation-delay: .5s;
	}

	.box p:nth-of-type(28) {
		animation-delay: .3s;
	}

	.box p:nth-of-type(29) {
		animation-delay: .5s;
	}

	.box p:nth-of-type(30) {
		animation-delay: .7s;
	}

	.box p:nth-of-type(31) {
		animation-delay: .3s;
	}

	.box p:nth-of-type(32) {
		animation-delay: .7s;
	}

	.box p:nth-of-type(33) {
		animation-delay: .3s;
	}

	.box p:nth-of-type(34) {
		animation-delay: .5s;
	}

	.box p:nth-of-type(35) {
		animation-delay: .7s;
	}

	.box p:nth-of-type(36) {
		animation-delay: .3s;
	}

	.box p:nth-of-type(37) {
		animation-delay: .5s;
	}

	.box p:nth-of-type(38) {
		animation-delay: .7s;
	}

	.box p:nth-of-type(39) {
		animation-delay: .3s;
	}

	.box p:nth-of-type(40) {
		animation-delay: .5s;
	}


	@keyframes Mymove1 {
		0% {
			height: 5px;
		}

		50% {
			height: 26px;
		}

		100% {
			height: 10px;
		}
	}

	@keyframes Mymove2 {
		0% {
			height: 10px;
		}

		50% {
			height: 26px;
		}

		100% {
			height: 13px;
		}
	}
</style>
