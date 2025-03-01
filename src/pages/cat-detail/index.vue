<template>
  <view class="cat-detail">
    <!-- 图片轮播 -->
    <swiper class="swiper" circular :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000">
      <swiper-item v-for="(image, index) in catImages" :key="index">
        <image :src="image" mode="aspectFill" class="swiper-image" />
      </swiper-item>
    </swiper>

    <!-- 基本信息 -->
    <view class="info-section">
      <view class="cat-name">{{ catInfo.name }}</view>
      <view class="cat-tags">
        <text class="tag">{{ catInfo.breed }}</text>
        <text class="tag">{{ catInfo.age }}岁</text>
        <text class="tag">{{ catInfo.gender === 0 ? '公' : '母' }}</text>
      </view>

      <!-- 详细信息 -->
      <view class="info-list">
        <view class="info-item">
          <text class="label">体重：</text>
          <text class="value">{{ catInfo.weight }}kg</text>
        </view>
        <view class="info-item">
          <text class="label">毛色：</text>
          <text class="value">{{ catInfo.color }}</text>
        </view>
        <view class="info-item">
          <text class="label">绝育情况：</text>
          <text class="value">{{ catInfo.sterilization === 1 ? '已绝育' : '未绝育' }}</text>
        </view>
        <view class="info-item">
          <text class="label">疫苗接种：</text>
          <text class="value">{{ catInfo.vaccine_status }}</text>
        </view>
      </view>

      <!-- 性格描述 -->
      <view class="section">
        <view class="section-title">性格特点</view>
        <view class="section-content">{{ catInfo.character }}</view>
      </view>

      <!-- 健康状况 -->
      <view class="section">
        <view class="section-title">健康状况</view>
        <view class="section-content">{{ catInfo.health_status }}</view>
      </view>

      <!-- 认领要求 -->
      <view class="section">
        <view class="section-title">认领要求</view>
        <view class="section-content">{{ catInfo.adoption_requirements }}</view>
      </view>

      <!-- 认领费用 -->
      <view class="adoption-fee">
        <text class="fee-label">认领费用</text>
        <text class="fee-amount">¥{{ catInfo.adoption_fee }}</text>
      </view>
    </view>

    <!-- 底部操作栏 -->
    <view class="bottom-bar">
      <view class="favorite-btn" @click="toggleFavorite">
        <text class="iconfont" :class="{ 'icon-favorite': isFavorite, 'icon-favorite-o': !isFavorite }"></text>
        <text>{{ isFavorite ? '已收藏' : '收藏' }}</text>
      </view>
      <button class="adoption-btn" @click="startAdoption" :disabled="catInfo.status !== 0">{{ getAdoptionButtonText }}</button>
    </view>

    <!-- 相似猫咪推荐 -->
    <view class="similar-cats">
      <view class="section-title">相似猫咪</view>
      <scroll-view scroll-x class="similar-list">
        <view class="similar-item" v-for="cat in similarCats" :key="cat.id" @click="navigateToCat(cat.id)">
          <image :src="cat.cover_image" mode="aspectFill" class="similar-image" />
          <text class="similar-name">{{ cat.name }}</text>
        </view>
      </scroll-view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      catId: null,
      catImages: [
        '/static/banner1.jpg',
        '/static/banner2.jpg',
        '/static/banner3.jpg'
      ],
      catInfo: {
        name: '小花',
        breed: '中华田园猫',
        age: 2,
        gender: 1,
        weight: 3.5,
        color: '三花',
        character: '温顺亲人，喜欢玩耍',
        health_status: '已做全面体检，健康状况良好',
        vaccine_status: '已完成全部疫苗接种',
        sterilization: 1,
        adoption_requirements: '要求有稳定的居住环境，有养猫经验优先',
        adoption_fee: 199,
        status: 0 // 0-可认领,1-认领中,2-已认领,3-暂不可认领
      },
      isFavorite: false,
      similarCats: [
        {
          id: 2,
          name: '奥利奥',
          cover_image: '/static/cat2.jpg'
        },
        {
          id: 3,
          name: '咪咪',
          cover_image: '/static/cat3.jpg'
        }
      ]
    }
  },
  onLoad(options) {
    // 获取路由参数中的猫咪ID
    this.catId = options.id
    // TODO: 根据catId从后端获取猫咪详细信息
    this.loadCatDetail()
  },
  computed: {
    getAdoptionButtonText() {
      const status = this.catInfo.status
      switch(status) {
        case 0:
          return '我要认领'
        case 1:
          return '认领中'
        case 2:
          return '已被认领'
        case 3:
          return '暂不可认领'
        default:
          return '我要认领'
      }
    }
  },
  methods: {
    loadCatDetail() {
      // TODO: 调用后端API获取猫咪详情
      console.log('加载猫咪ID:', this.catId, '的详细信息')
    },
    toggleFavorite() {
      this.isFavorite = !this.isFavorite
      // TODO: 调用后端API更新收藏状态
    },
    startAdoption() {
      if (this.catInfo.status === 0) {
        uni.navigateTo({
          url: `/pages/adoption-apply/index?catId=${this.catId}`
        })
      }
    },
    navigateToCat(id) {
      // 跳转到其他猫咪详情页
      uni.redirectTo({
        url: `/pages/cat-detail/index?id=${id}`
      })
    }
  }
}
</script>

<style lang="scss">
  .cat-detail {
    min-height: 100vh;
    background-color: #FFFAF0;
    padding-bottom: 100rpx;
  }

  .swiper {
    height: 500rpx;
    
    .swiper-image {
      width: 100%;
      height: 100%;
    }
  }

  .info-section {
    background-color: #fff;
    padding: 30rpx;
    margin-bottom: 20rpx;

    .cat-name {
      font-size: 44rpx;
      font-weight: 600;
      margin-bottom: 24rpx;
      color: #333;
      letter-spacing: 1rpx;
    }

    .cat-tags {
      display: flex;
      flex-wrap: wrap;
      gap: 16rpx;
      margin-bottom: 30rpx;

      .tag {
        background-color: #ffeef1;
        color: #ff4d6a;
        padding: 10rpx 24rpx;
        border-radius: 24rpx;
        font-size: 24rpx;
        font-weight: 500;
        margin-right: 16rpx;
        box-shadow: 0 4rpx 8rpx rgba(255, 77, 106, 0.1);
      }
    }

    .info-list {
      margin-bottom: 30rpx;

      .info-item {
        display: flex;
        margin-bottom: 20rpx;
        align-items: center;

        .label {
          color: #999;
          width: 160rpx;
          font-size: 26rpx;
        }

        .value {
          color: #333;
          flex: 1;
          font-size: 28rpx;
          font-weight: 500;
        }
      }
    }
  }

  .section {
    margin-bottom: 30rpx;

    .section-title {
      font-size: 32rpx;
      font-weight: 600;
      margin-bottom: 20rpx;
      color: #ff4d6a;
      position: relative;
      padding-left: 24rpx;
      &::before {
        content: '';
        position: absolute;
        left: 0;
        top: 50%;
        transform: translateY(-50%);
        width: 8rpx;
        height: 32rpx;
        background: #ff4d6a;
        border-radius: 4rpx;
      }
    }

    .section-content {
      font-size: 28rpx;
      color: #666;
      line-height: 1.8;
      letter-spacing: 0.5rpx;
      padding: 0 10rpx;
    }
  }

  .adoption-fee {
    display: flex;
    align-items: center;
    margin-top: 30rpx;

    .fee-label {
      font-size: 32rpx;
      color: #666;
      font-weight: 500;
    }

    .fee-amount {
      font-size: 48rpx;
      color: #ff4d6a;
      font-weight: 600;
      margin-left: 24rpx;
      text-shadow: 0 2rpx 4rpx rgba(255, 77, 106, 0.2);
    }
  }

  .bottom-bar {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    height: 100rpx;
    background-color: #fff;
    display: flex;
    align-items: center;
    padding: 0 30rpx;
    box-shadow: 0 -2rpx 10rpx rgba(0, 0, 0, 0.05);

    .favorite-btn {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-right: 30rpx;
      font-size: 24rpx;
      color: #666;

      .iconfont {
        font-size: 40rpx;
        margin-bottom: 4rpx;

        &.icon-favorite {
          color: #ff4d6a;
        }
      }
    }

    .adoption-btn {
      flex: 1;
      height: 72rpx;
      background-color: #ff4d6a;
      color: #fff;
      border-radius: 36rpx;
      font-size: 28rpx;
      display: flex;
      align-items: center;
      justify-content: center;

      &:disabled {
        background-color: #ccc;
      }
    }
  }

  .similar-cats {
    background-color: #fff;
    padding: 30rpx;
    margin-top: 20rpx;

    .similar-list {
      white-space: nowrap;
      margin-top: 20rpx;

      .similar-item {
        display: inline-block;
        margin-right: 20rpx;
        width: 200rpx;

        .similar-image {
          width: 200rpx;
          height: 200rpx;
          border-radius: 12rpx;
        }

        .similar-name {
          font-size: 26rpx;
          color: #333;
          margin-top: 10rpx;
          white-space: nowrap;
          overflow: hidden;
          text-overflow: ellipsis;
        }
      }
    }
  }

</style>