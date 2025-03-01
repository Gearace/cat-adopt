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
          <view class="cat-info" :class="{'female-cat': cat.gender === 1, 'male-cat': cat.gender === 0}">
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
          cover: '/static/cat1.jpg',
          gender: 1 // 母猫
        },
        {
          id: 2,
          name: '奥利奥',
          breed: '英短',
          cover: '/static/cat2.jpg',
          gender: 0 // 公猫
        },
        {
          id: 3,
          name: '咪咪',
          breed: '美短',
          cover: '/static/cat3.jpg',
          gender: 1 // 母猫
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
  background: linear-gradient(180deg, #ffffff 0%, #f5f5f5 100%);
  position: relative;
  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-image: url('data:image/png;base64,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');
    opacity: 0.05;
    pointer-events: none;
  }
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

.category-section {
  display: flex;
  justify-content: space-around;
  padding: 40rpx 20rpx;
  background: linear-gradient(90deg, #f783a6 0%, #f99bb6 100%);
  margin: 30rpx 20rpx;
  border-radius: 20rpx;
  border: 2rpx solid #FFB6C1;
  box-shadow: 0 4rpx 20rpx rgba(255, 77, 106, 0.15);
}

.category-icon {
  width: 90rpx;
  height: 90rpx;
  margin-bottom: 12rpx;
  padding: 15rpx;
  background-color: #ffffff;
  border-radius: 50%;
  border: 2rpx solid #D4E6F5;
}

.category-name {
  font-size: 26rpx;
  color: #ffffff;
  font-weight: 500;
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
    box-shadow: 0 4rpx 20rpx rgba(255, 77, 106, 0.15);
  }
  .cat-image {
    width: 100%;
    height: 300rpx;
    display: block;
  }
  .cat-info {
    padding: 16rpx;
    &.female-cat {
      background: linear-gradient(90deg, #f783a6 0%, #f99bb6 100%);
      border: 2rpx solid rgba(255, 77, 106, 0.8);
      .cat-name {
        color: #ffffff;
      }
      .cat-breed {
        color: #ffffff;
      }
    }
    &.male-cat {
      background: #87CEEB;
      border: 2rpx solid #87CEEB;
      .cat-name {
        color: #4d4d4d;
      }
      .cat-breed {
        color: #4d4d4d;
      }
    }
    .cat-name {
      font-size: 28rpx;
      font-weight: 600;
      margin-bottom: 8rpx;
    }
    .cat-breed {
      font-size: 24rpx;
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
