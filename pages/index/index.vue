<template>
	<view>
		<!-- 菜单导航栏 -->
		<view>
			<scroll-view scroll-x class="bg-white nav" scroll-with-animation :scroll-left="scrollLeft">
				<view class="cu-item" :class="index==TabCur?'text-green cur':''" v-for="(item,index) in menuList" :key="index" @tap="tabSelect"
				 :data-id="index" :data-menuid="item.id">
					{{item.name}}
				</view>
			</scroll-view>
		</view>

		<!-- 文章列表 -->
		<view>
			<view class="cu-card article no-card margin-bottom-xs solid-top" v-for="(item,index) in articleList" :key="index">
				<view class="cu-item shadow">
					<view class="title">
						<view class="text-cut">{{item.title}}</view>
					</view>
					<view class="content">
						<image class="img" v-if="item.images.length>0" :src="item.images[0]"  style="background-color: #007AFF;"></image>
						<view class="desc">
							<view class="text-content">{{item.content}}</view>

						</view>
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
				TabCur: 0, // 当前激活的菜单索引
				scrollLeft: 0, //偏移量

				menuList: [], //菜单列表
				articleList: [] // 默认文章列表
			}
		},
		onLoad() {
			this.doGetDefaultArticleAndMenuList()
		},
		methods: {
			// tab切换
			tabSelect(e) {
				this.TabCur = e.currentTarget.dataset.id;
				this.scrollLeft = (e.currentTarget.dataset.id - 1) * 60
			},

			// 获取所有菜单和默认文章
			doGetDefaultArticleAndMenuList() {
				uni.request({
					url: 'https://test.meitandata.com/appapi/article/default/init',
					method: 'GET',
					header: {
						'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJyb2xlcyI6W10sImlwIjoiMTExLjg1LjE5OC41MyIsInVzZXJJZCI6IjE3NDAiLCJhY3Rpb25zIjpbXSwiaWF0IjoxNTkwNTY4Mjg3LCJwbGF0Zm9ybSI6Im1pbmkifQ.LgQIgw0weKSWOSN1WeBAwVhSypdr4lUyCnYsSgaI_Sc',
						'Platform': 'mini',
						'Accept': 'application/json'
					},
					data: {},
					success: res => {
						this.menuList = res.data.data.allMenuList;
						this.articleList = res.data.data.defaultArticleList;
					}
				});
			}
		}
	}
</script>

<style>
.img{
	display: flex;
	justify-content: left;
}
</style>
