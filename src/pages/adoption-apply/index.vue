<template>
  <view class="adoption-apply">
    <view class="header">
      <text class="title">认领申请</text>
      <text class="subtitle">请如实填写以下信息，我们将认真审核您的申请</text>
    </view>

    <view class="notice-section">
      <view class="notice-title">认领须知</view>
      <view class="notice-content">
        <view class="notice-item">1. 认领人需年满18周岁，有完全民事行为能力</view>
        <view class="notice-item">2. 需要具备稳定的居住环境和经济条件</view>
        <view class="notice-item">3. 承诺善待猫咪，提供良好的生活环境</view>
        <view class="notice-item">4. 认领后需定期反馈猫咪状况</view>
      </view>
    </view>

    <view class="form-section">
      <form @submit="handleSubmit">
        <view class="form-group">
          <text class="label">居住条件</text>
          <input 
            class="input" 
            type="text" 
            v-model="formData.livingCondition"
            placeholder="请描述您的居住环境（面积、是否租房等）"
          />
        </view>

        <view class="form-group">
          <text class="label">养猫经验</text>
          <textarea 
            class="textarea" 
            v-model="formData.experience"
            placeholder="请描述您的养猫经验"
          />
        </view>

        <view class="form-group">
          <text class="label">家庭成员情况</text>
          <textarea 
            class="textarea" 
            v-model="formData.familyMembers"
            placeholder="请描述您的家庭成员情况，是否都同意养猫"
          />
        </view>

        <view class="form-group">
          <text class="label">认领原因</text>
          <textarea 
            class="textarea" 
            v-model="formData.reason"
            placeholder="请说明您想认领这只猫咪的原因"
          />
        </view>

        <button class="submit-btn" form-type="submit">提交申请</button>
      </form>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        livingCondition: '',
        experience: '',
        familyMembers: '',
        reason: ''
      }
    }
  },
  methods: {
    handleSubmit() {
      // 表单验证
      if (!this.formData.livingCondition || 
          !this.formData.experience || 
          !this.formData.familyMembers || 
          !this.formData.reason) {
        uni.showToast({
          title: '请填写完整信息',
          icon: 'none'
        })
        return
      }

      // TODO: 调用后端API提交申请
      uni.showLoading({
        title: '提交中...'
      })

      // 模拟API调用
      setTimeout(() => {
        uni.hideLoading()
        uni.showToast({
          title: '申请提交成功',
          icon: 'success'
        })
        // 跳转到申请状态页面
        setTimeout(() => {
          uni.navigateTo({
            url: '/pages/adoption-status/index'
          })
        }, 1500)
      }, 2000)
    }
  }
}
</script>

<style lang="scss">
.adoption-apply {
  padding: 30rpx;
  background-color: #fff;

  .header {
    text-align: center;
    margin-bottom: 40rpx;

    .title {
      font-size: 36rpx;
      font-weight: bold;
      color: #333;
    }

    .subtitle {
      font-size: 26rpx;
      color: #666;
      margin-top: 10rpx;
    }
  }

  .notice-section {
    background-color: #fff5f7;
    padding: 20rpx;
    border-radius: 12rpx;
    margin-bottom: 40rpx;

    .notice-title {
      font-size: 30rpx;
      font-weight: bold;
      color: #ff4d6a;
      margin-bottom: 20rpx;
    }

    .notice-content {
      .notice-item {
        font-size: 26rpx;
        color: #666;
        line-height: 1.6;
        margin-bottom: 10rpx;
        padding-left: 20rpx;
      }
    }
  }

  .form-section {
    .form-group {
      margin-bottom: 30rpx;

      .label {
        display: block;
        font-size: 28rpx;
        color: #333;
        margin-bottom: 15rpx;
      }

      .input {
        width: 100%;
        height: 80rpx;
        border: 2rpx solid #eee;
        border-radius: 8rpx;
        padding: 0 20rpx;
        font-size: 28rpx;
        background-color: #f8f8f8;

        &:focus {
          border-color: #ff4d6a;
        }
      }

      .textarea {
        width: 100%;
        height: 160rpx;
        border: 2rpx solid #eee;
        border-radius: 8rpx;
        padding: 20rpx;
        font-size: 28rpx;
        background-color: #f8f8f8;

        &:focus {
          border-color: #ff4d6a;
        }
      }
    }

    .submit-btn {
      width: 100%;
      height: 88rpx;
      line-height: 88rpx;
      text-align: center;
      background-color: #ff4d6a;
      color: #fff;
      font-size: 32rpx;
      border-radius: 44rpx;
      margin-top: 40rpx;

      &:active {
        opacity: 0.8;
      }
    }
  }
}
</style>