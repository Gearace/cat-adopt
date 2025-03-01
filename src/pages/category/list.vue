<template>
  <view class="category-list">
    <!-- 筛选区域 -->
    <view class="filter-section">
      <!-- 年龄分类 -->
      <scroll-view scroll-x class="filter-scroll" show-scrollbar="false">
        <view class="filter-tags">
          <view 
            class="filter-tag" 
            :class="{ active: currentAge === age.value }" 
            v-for="age in ages" 
            :key="age.value"
            @tap="selectAge(age.value)"
          >
            {{ age.label }}
          </view>
        </view>
      </scroll-view>
      <!-- 条件筛选 -->
      <scroll-view scroll-x class="filter-scroll" show-scrollbar="false">
        <view class="filter-tags">
          <view 
            class="filter-tag" 
            :class="{ active: currentFilter === filter.value }" 
            v-for="filter in filters" 
            :key="filter.value"
            @tap="selectFilter(filter.value)"
          >
            {{ filter.label }}
          </view>
        </view>
      </scroll-view>
      <!-- 品种筛选 -->
      <scroll-view scroll-x class="breed-scroll" show-scrollbar="false">
        <view class="breed-list">
          <view 
            class="breed-item" 
            :class="{ active: currentBreed === breed.value }" 
            v-for="breed in breeds" 
            :key="breed.value"
            @tap="selectBreed(breed.value)"
          >
            <image :src="breed.icon" mode="aspectFit" class="breed-icon" />
            <text class="breed-name">{{ breed.label }}</text>
          </view>
        </view>
      </scroll-view>
    </view>

    <!-- 列表区域 -->
    <scroll-view 
      class="cat-scroll" 
      scroll-y 
      @scrolltolower="loadMore"
      refresher-enabled
      :refresher-triggered="isRefreshing"
      @refresherrefresh="onRefresh"
    >
      <view class="cat-grid">
        <view 
          class="cat-item" 
          v-for="cat in catList" 
          :key="cat.id"
          @tap="navigateToCatDetail(cat.id)"
        >
          <image :src="cat.cover" mode="aspectFill" class="cat-image" />
          <view class="cat-info" :class="{'female-cat': cat.gender === 1, 'male-cat': cat.gender === 0}">
            <text class="cat-name">{{cat.name}}</text>
            <view class="cat-tags">
              <text class="tag">{{cat.breed}}</text>
              <text class="tag">{{cat.age}}岁</text>
              <text class="tag">{{cat.gender === 0 ? '公' : '母'}}</text>
            </view>
          </view>
        </view>
      </view>
      <!-- 加载更多提示 -->
      <view class="loading-more" v-if="hasMore">
        <text>加载中...</text>
      </view>
      <view class="no-more" v-else>
        <text>没有更多了~</text>
      </view>
    </scroll-view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      categoryType: '',
      currentFilter: 'all',
      currentBreed: 'all',
      currentAge: 'all',
      isRefreshing: false,
      page: 1,
      hasMore: true,
      ages: [
        { label: '全年龄', value: 'all', icon: '/static/cat-all.png' },
        { label: '幼猫', value: 'kitten', icon: '/static/cat-kitten.png' },
        { label: '成年猫', value: 'adult', icon: '/static/cat-adult.png' },
        { label: '老年猫', value: 'senior', icon: '/static/cat-senior.png' }
      ],
      breeds: [
        { label: '全品种', value: 'all', icon: '/static/cat-all.png' },
        { label: '蓝白猫', value: 'blue-white', icon: '/static/breeds/blue-white.png' },
        { label: '蓝猫', value: 'bluecat', icon: '/static/breeds/bluecat.png' },
        { label: '银渐层', value: 'silver-tabby', icon: '/static/breeds/silver-tabby.png' },
        { label: '金渐层', value: 'golden-tabby', icon: '/static/breeds/golden-tabby.png' },
        { label: '美短', value: 'american-shorthair', icon: '/static/breeds/american-shorthair.png' },
        { label: '暹罗猫', value: 'siamese-kitten', icon: '/static/breeds/siamese-kitten.png' },
        { label: '加菲猫', value: 'garfield', icon: '/static/breeds/garfield.png' },
        { label: '布偶猫', value: 'ragdoll', icon: '/static/breeds/ragdoll.png' },
        { label: '缅因猫', value: 'mainecoon', icon: '/static/breeds/mainecoon.png' },
        { label: '狮子猫', value: 'lioncat', icon: '/static/breeds/lioncat.png' },
        { label: '大橘猫', value: 'gingercat', icon: '/static/breeds/gingercat.png' },
        { label: '奶牛猫', value: 'cowcat', icon: '/static/breeds/cowcat.png' },
        { label: '狸花猫', value: 'tabby', icon: '/static/breeds/tabby.png' },
        { label: '黑猫', value: 'blackcat', icon: '/static/breeds/blackcat.png' },
        { label: '三花猫', value: 'calico', icon: '/static/breeds/calico.png' },
        { label: '玳瑁猫', value: 'tortie', icon: '/static/breeds/tortie.png' },
        { label: '阿比西尼亚猫', value: 'abyssinian', icon: '/static/breeds/abyssinian.png' },
        { label: '斯芬克斯无毛猫', value: 'sphynx', icon: '/static/breeds/sphynx.png' }
      ],
      filters: [
        { label: '不限', value: 'all' },
        { label: '公猫', value: 'male' },
        { label: '母猫', value: 'female' },
        { label: '已绝育', value: 'sterilized' },
        { label: '未绝育', value: 'unsterilized' }
      ],
      catList: [
        {
          id: 1,
          name: '小花',
          breed: '中华田园猫',
          age: 2,
          gender: 1,
          cover: '/static/cat1.jpg'
        },
        {
          id: 2,
          name: '奥利奥',
          breed: '英短',
          age: 1,
          gender: 0,
          cover: '/static/cat2.jpg'
        },
        {
          id: 3,
          name: '咪咪',
          breed: '美短',
          age: 3,
          gender: 1,
          cover: '/static/cat3.jpg'
        }
      ]
    }
  },
  onLoad(options) {
    // 获取路由参数中的分类类型
    this.categoryType = options.type
    uni.setNavigationBarTitle({
      title: this.categoryType
    })
    this.loadCatList()
  },
  methods: {
    selectBreed(value) {
      this.currentBreed = value
      this.page = 1
      this.hasMore = true
      this.catList = []
      this.loadCatList()
    },
    selectAge(value) {
      this.currentAge = value
      this.page = 1
      this.hasMore = true
      this.catList = []
      this.loadCatList()
    },
    selectFilter(value) {
      this.currentFilter = value
      this.page = 1
      this.hasMore = true
      this.catList = []
      this.loadCatList()
    },
    loadCatList() {
      // TODO: 调用后端API获取猫咪列表
      console.log('加载猫咪列表:', {
        type: this.categoryType,
        filter: this.currentFilter,
        page: this.page
      })
    },
    async onRefresh() {
      this.isRefreshing = true
      this.page = 1
      this.hasMore = true
      await this.loadCatList()
      this.isRefreshing = false
    },
    loadMore() {
      if (!this.hasMore) return
      this.page++
      this.loadCatList()
    },
    navigateToCatDetail(id) {
      uni.navigateTo({
        url: `/pages/cat-detail/index?id=${id}`
      })
    }
  }
}
</script>

<style>
.category-list {
  min-height: 100vh;
  background: linear-gradient(180deg, #ffffff 0%, #f5f5f5 100%);
  position: relative;
  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-image: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAMAAAAp4XiDAAAAUVBMVEWFhYWDg4N3d3dtbW17e3t1dXWBgYGHh4d5eXlzc3OLi4ubm5uVlZWPj4+NjY19fX2JiYl/f39ra2uRkZGZmZlpaWmXl5dvb29xcXGTk5NnZ2c8TV1mAAAAG3RSTlNAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEAvEOwtAAAFVklEQVR4XpWWB67c2BUFb3g557T/hRo9/WUMZHlgr4Bg8Z4qQgQJlHI4A8SzFVrapvmTF9O7dmYRFZ60YiBhJRCgh1FYhiLAmdvX0CzTOpNE77ME0Zty/nWWzchDtiqrmQDeuv3powQ5ta2eN0FY0InkqDD73lT9c9lEzwUNqgFHs9VQce3TVClFCQrSTfOiYkVJQBmpbq2L6iZavPnAPcoU0dSw0SUTqz/GtrGuXfbyyBniKykOWQWGqwwMA7QiYAxi+IlPdqo+hYHnUt5ZPfnsHJyNiDtnpJyayNBkF6cWoYGAMY92U2hXHF/C1M8uP/ZtYdiuj26UdAdQQSXQErwSOMzt/XWRWAz5GuSBIkwG1H3FabJ2OsUOUhGC6tK4EMtJO0ttC6IBD3kM0ve0tJwMdSfjZo+EEISaeTr9P3wYrGjXqyC1krcKdhMpxEnt5JetoulscpyzhXN5FRpuPHvbeQaKxFAEB6EN+cYN6xD7RYGpXpNndMmZgM5Dcs3YSNFDHUo2LGfZuukSWyUYirJAdYbF3MfqEKmjM+I2EfhA94iG3L7uKrR+GdWD73ydlIB+6hgref1QTlmgmbM3/LeX5GI1Ux1RWpgxpLuZ2+I+IjzZ8wqE4nilvQdkUdfhzI5QDWy+kw5Wgg2pGpeEVeCCA7b85BO3F9DzxB3cdqvBzWcmzbyMiqhzuYqtHRVG2y4x+KOlnyqla8AoWWpuBoYRxzXrfKuILl6SfiWCbjxoZJUaCBj1CjH7GIaDbc9kqBY3W/Rgjda1iqQcOJu2WW+76pZC9QG7M00dffe9hNnseupFL53r8F7YHSwJWUKP2q+k7RdsxyOB11n0xtOvnW4irMMFNV4H0uqwS5ExsmP9AxbDTc9JwgneAT5vTiUSm1E7BSflSt3bfa1tv8Di3R8n3Af7MNWzs49hmauE2wP+ttrq+AsWpFG2awvsuOqbipWHgtuvuaAE+A1Z/7gC9hesnr+7wqCwG8c5yAg3AL1fm8T9AZtp/bbJGwl1pNrE7RuOX7PeMRUERVaPpEs+yqeoSmuOlokqw49pgomjLeh7icHNlG19yjs6XXOMedYm5xH2YxpV2tc0Ro2jJfxC50ApuxGob7lMsxfTbeUv07TyYxpeLucEH1gNd4IKH2LAg5TdVhlCafZvpskfncCfx8pOhJzd76bJWeYFnFciwcYfubRc12Ip/ppIhA1/mSZ/RxjFDrJC5xifFjJpY2Xl5zXdguFqYyTR1zSp1Y9p+tktDYYSNflcxI0iyO4TPBdlRcpeqjK/piF5bklq77VSEaA+z8qmJTFzIWiitbnzR794USKBUaT0NTEsVjZqLaFVqJoPN9ODG70IPbfBHKK+/q/AWR0tJzYHRULOa4MP+W/HfGadZUbfw177G7j/OGbIs8TahLyynl4X4RinF793Oz+BU0saXtUHrVBFT/DnA3ctNPoGbs4hRIjTok8i+algT1lTHi4SxFvONKNrgQFAq2/gFnWMXgwffgYMJpiKYkmW3tTg3ZQ9Jq+f8XN+A5eeUKHWvJWJ2sgJ1Sop+wwhqFVijqWaJhwtD8MNlSBeWNNWTa5Z5kPZw5+LbVT99wqTdx29lMUH4OIG/D86ruKEauBjvH5xy6um/Sfj7ei6UUVk4AIl3MyD4MSSTOFgSwsH/QJWaQ5as7ZcmgBZkzjjU1UrQ74ci1gWBCSGHtuV1H2mhSnO3Wp/3fEV5a+4wz//6qy8JxjZsmxxy5+4w9CDNJY09T072iKG0EnOS0arEYgXqYnXcYHwjTtUNAcMelOd4xpkoqiTYICWFq0JSiPfPDQdnt+4/wuqcXY47QILbgAAAABJRU5ErkJggg==');
    opacity: 0.05;
    pointer-events: none;
  }
}

.filter-section {
  position: sticky;
  top: 0;
  z-index: 100;
  background: linear-gradient(90deg, #f783a6 0%, #f99bb6 100%);
  padding: 20rpx 0;
  box-shadow: 0 4rpx 16rpx rgba(255, 77, 106, 0.2);
}

.breed-scroll {
  white-space: nowrap;
  padding: 0 20rpx;
  margin-bottom: 20rpx;
}

.breed-list {
  display: inline-flex;
  padding: 10rpx 0;
}

.breed-item {
  display: inline-flex;
  flex-direction: column;
  align-items: center;
  margin-right: 30rpx;
  opacity: 0.6;
  transition: all 0.3s;
}

.breed-item.active {
  opacity: 1;
}

.breed-icon {
  width: 100rpx;
  height: 100rpx;
  margin-bottom: 8rpx;
  background-color: rgba(255, 255, 255, 0.9);
  border-radius: 50%;
  padding: 0;
}

.breed-name {
  font-size: 24rpx;
  color: #ffffff;
  white-space: nowrap;
}

.filter-scroll {
  white-space: nowrap;
  padding: 0 20rpx;
}

.filter-tags {
  display: inline-flex;
  padding: 10rpx 0;
}

.filter-tag {
  display: inline-block;
  padding: 12rpx 28rpx;
  margin-right: 24rpx;
  background-color: rgba(255, 255, 255, 0.25);
  color: #ffffff;
  border-radius: 30rpx;
  font-size: 28rpx;
  font-weight: 500;
  transition: all 0.3s;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}

.filter-tag.active {
  background-color: #ffffff;
  color: #ff4d6a;
  font-weight: 600;
  box-shadow: 0 2rpx 8rpx rgba(255, 77, 106, 0.2);
}

.breed-name {
  font-size: 24rpx;
  color: #ffffff;
  white-space: nowrap;
  font-weight: 500;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}

.cat-scroll {
  height: calc(100vh - 100rpx);
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

    .cat-image {
      width: 100%;
      height: 300rpx;
      display: block;
    }

    .cat-info {
      padding: 16rpx;
      background: rgba(255, 77, 106, 0.8);
      &.female-cat {
        background: linear-gradient(90deg, #f783a6 0%, #f99bb6 100%);
        border: 2rpx solid rgba(255, 77, 106, 0.8);
        .cat-name {
          color: #ffffff;
        }
        .cat-tags {
          .tag {
            color: #ffffff;
          }
        }
      }
      &.male-cat {
        background: #87CEEB;
        border: 2rpx solid #87CEEB;
        .cat-name {
          color: #4d4d4d;
        }
        .cat-tags {
          .tag {
            color: #4d4d4d;
          }
        }
      }
      .cat-name {
        font-size: 28rpx;
        font-weight: 600;
        margin-bottom: 8rpx;
      }

      .cat-tags {
        display: flex;
        flex-wrap: wrap;
        gap: 8rpx;

        .tag {
          font-size: 22rpx;
          background: rgba(255, 255, 255, 0.2);
          padding: 4rpx 12rpx;
          border-radius: 12rpx;
        }
      }
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
  margin-bottom: 10rpx;
}

.cat-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 10rpx;
}

.tag {
  font-size: 22rpx;
  color: #FF69B4;
  background-color: #FFF0F5;
  padding: 4rpx 12rpx;
  border-radius: 6rpx;
}

.loading-more,
.no-more {
  text-align: center;
  padding: 30rpx 0;
  color: #999999;
  font-size: 24rpx;
}
</style>