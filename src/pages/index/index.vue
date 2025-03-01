<template>
  <view class="container">
    <!-- 轮播图 -->
    <swiper class="banner" circular :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000">
      <swiper-item v-for="(item, index) in bannerList" :key="index" @tap="navigateToCatDetail(item.catId)">
        <image :src="item.image" mode="aspectFill" class="banner-image" />
      </swiper-item>
    </swiper>

    <!-- 分类导航 -->
    <view class="category-section">
      <view class="category-item" v-for="(item, index) in categories" :key="index" @tap="navigateToCategory(item)">
        <image :src="item.icon" mode="aspectFit" class="category-icon" />
        <text class="category-name">{{item.name}}</text>
      </view>
    </view>

    <!-- 猫咪列表 -->
    <view class="cat-list">
      <view class="section-title">推荐猫咪</view>
      <view class="cat-grid">
        <view class="cat-item" v-for="(cat, index) in catList" :key="index" @tap="navigateToCatDetail(cat.id)">
          <image :src="cat.cover" mode="aspectFill" class="cat-image" />
          <view class="cat-info">
            <text class="cat-name">{{cat.name}}</text>
            <text class="cat-breed">{{cat.breed}}</text>
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
      bannerList: [
        { image: '/static/banner1.jpg', catId: 1 },
        { image: '/static/banner2.jpg', catId: 2 },
        { image: '/static/banner3.jpg', catId: 3 }
      ],
      categories: [
        { name: '全部猫咪', icon: '/static/cat-all.png' },
        { name: '幼猫', icon: '/static/cat-kitten.png' },
        { name: '成年猫', icon: '/static/cat-adult.png' },
        { name: '老年猫', icon: '/static/cat-senior.png' }
      ],
      catList: [
        {
          id: 1,
          name: '小花',
          breed: '中华田园猫',
          cover: '/static/cat1.jpg'
        },
        {
          id: 2,
          name: '奥利奥',
          breed: '英短',
          cover: '/static/cat2.jpg'
        },
        {
          id: 3,
          name: '咪咪',
          breed: '美短',
          cover: '/static/cat3.jpg'
        }
      ]
    }
  },
  methods: {
    navigateToCategory(category) {
      // 跳转到分类页面
      uni.navigateTo({
        url: `/pages/category/list?type=${category.name}`
      })
    },
    navigateToCatDetail(id) {
      // 跳转到猫咪详情页
      uni.navigateTo({
        url: `/pages/cat-detail/index?id=${id}`
      })
    }
  }
}
</script>

<style>
.container {
  padding-bottom: 20rpx;
  background-color: #FFFAF0;
}

.banner {
  width: 100%;
  height: 400rpx;
  border-radius: 0 0 30rpx 30rpx;
  overflow: hidden;
  box-shadow: 0 4rpx 16rpx rgba(255, 182, 193, 0.2);
}

.banner-image {
  width: 100%;
  height: 100%;
}

.category-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  transition: transform 0.3s;
}

.category-item:active {
  transform: scale(0.95);
}

.category-icon {
  width: 90rpx;
  height: 90rpx;
  margin-bottom: 12rpx;
  padding: 15rpx;
  background-color: #ffffff;
  border-radius: 50%;
}

.category-name {
  font-size: 26rpx;
  color: #ffffff;
  font-weight: 500;
}

.category-section {
  display: flex;
  justify-content: space-around;
  padding: 40rpx 20rpx;
  background-color: #ff4d6a;
  margin: 30rpx 20rpx;
  border-radius: 20rpx;
  box-shadow: 0 4rpx 20rpx rgba(255, 77, 106, 0.15);
}
.cat-list {
  padding: 0 20rpx;
}

.section-title {
  font-size: 34rpx;
  font-weight: bold;
  color: #FF69B4;
  background-color: #ffffff;
  margin: 30rpx 0;
  padding: 15rpx 20rpx;
  border-left: 8rpx solid #FFB6C1;
  border: 2rpx solid #FFB6C1;
  border-radius: 12rpx;
  box-shadow: 0 4rpx 12rpx rgba(255, 182, 193, 0.2);
}
.cat-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20rpx;
  padding: 20rpx;
  .cat-item {
    background: #fff;
    border-radius: 16rpx;
    overflow: hidden;
    border: 2rpx solid #ff4d6a;
    box-shadow: 0 4rpx 12rpx rgba(255, 77, 106, 0.1);
  }
  .cat-image {
    width: 100%;
    height: 300rpx;
    display: block;
  }
  .cat-info {
    padding: 16rpx;
    background: rgba(255, 77, 106, 0.8);
  .cat-name {
    font-size: 28rpx;
    font-weight: 600;
    color: #ffffff;
    margin-bottom: 8rpx;
  }
  .cat-breed {
    font-size: 24rpx;
    color: #ffffff;
    opacity: 0.9;
  }
  }
}
.cat-item:active {
  transform: scale(0.98);
}

.cat-image {
  width: 100%;
  height: 320rpx;
}

.cat-info {
  padding: 20rpx;
}

.cat-name {
  font-size: 30rpx;
  font-weight: bold;
  color: #FF69B4;
}

.cat-breed {
  font-size: 26rpx;
  color: #FFB6C1;
  margin-top: 6rpx;
}
</style>
