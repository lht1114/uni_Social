<template>
	<view class="content">
		<!-- <image class="logo" src="/static/logo.png"></image> -->
		<view class="text-area">
			<text class="title">{{title}}</text>
		</view>
		<view class="base">
			<view class="base_list" v-for="item in articleData" :key="item.id">
				<view class="list_top">
					{{ item.title }}
				</view>
				<view class="list_content">
					{{ item.content }}
				</view>
				<view class="list_bottom">
					<text>{{ item.author }}</text>
					<text>{{ item.createTime }}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '文章列表',
				articleData: []
			}
		},
		onPullDownRefresh: function() {
			this.getArticle()
		},
		onLoad() {
			this.getArticle()
		},
		methods: {
			getArticle() {
				uni.showNavigationBarLoading();
				uni.request({
					url: 'http://127.0.0.1:8888/article/selectArticle',
					method: 'POST',
					data: {
						title: ''
					},
					success: (res) => {
						console.log(res)
						this.articleData = res.data
						uni.hideNavigationBarLoading();
						uni.stopPullDownRefresh()
					},
					fail: () => {
						uni.hideNavigationBarLoading();
						uni.stopPullDownRefresh()
					}
				})
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
	.base {
		width: calc(100% - 40rpx);
		padding: 20rpx;
	}
	.base .base_list {
		padding: 20rpx;
		border-radius: 20rpx;
		background-color: #cccccc;
		margin-bottom: 20rpx;
	}
	.base_list .list_top {
		font-size: 30rpx;
		font-weight: bold;
		text-align: center;
	}
	.base_list .list_content {
		margin: 25rpx 0;
		text-indent: 2rem;
	}
	.base_list .list_bottom {
		width: 100%;
		display: flex;
		font-size: 26rpx;
		align-items: center;
		justify-content: space-between;
	}
</style>
