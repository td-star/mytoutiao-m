<template>
    <div class="login-container">
        <van-nav-bar
          title="登陆/注册"
          left-arrow
          @click-left="$router.back()"
          class="app-nav-bar"
        />
        <van-form
          :show-error="false"
          :show-error-message="false"
          validate-first
          ref="login-form"
          @submit="onLogin"
          @failed="onFailed"
        >
          <van-field
            v-model="user.mobile"
            icon-prefix="iconfont toutiao"
            left-icon="shouji"
            placeholder="请输入手机号"
            name="mobile"
            :rules="formRules.mobile"
          />
          <van-field
            v-model="user.code"
            clearable
            icon-prefix="iconfont toutiao"
            left-icon="yanzhengma"
            placeholder="请输入验证码"
            name="code"
            :rules="formRules.code"
          >
            <template #button>
              <van-count-down
                v-if="isCountDownShow"
                :time="1000 * 60"
                format="ss s"
                @finish="isCountDownShow = false"
              />
              <van-button
                v-else
                class="send-btn"
                size="small"
                round
                :loading="isSendSmsLoading"
                @click.prevent="onSendSms"
                >发送验证码</van-button>
            </template>
          </van-field>
          <div class="login-btn-wrap">
            <van-button class="login-btn" type="info" block>登陆</van-button>
          </div>
        </van-form>
    </div>
</template>

<script>
import { login, sendSms } from '@/api/user'
import { Toast } from 'vant'
export default {
  data () {
    return {
      user: {
        mobile: '',
        code: ''
      },
      formRules: {
        mobile: [
          { required: true, message: '请输入手机号' },
          { pattern: /^1[3|5|7|8|9]\d{9}$/, message: '手机号格式错误' }
        ],
        code: [
          { required: true, message: '请输入验证码' },
          { pattern: /^\d{6}$/, message: '验证码格式错误' }
        ]
      },
      isCountDownShow: false,
      isSendSmsLoading: false
    }
  },
  methods: {
    async onLogin () {
      Toast.loading({
        message: '登陆中...',
        forbidClick: true,
        duration: 0
      })
      try {
        const res = await login(this.user)
        Toast.success('登陆成功')
        this.$store.commit('setUser', res.data.data)
      } catch (err) {
        console.log(err)
        Toast.fail('登陆失败,请输入正确的手机号或验证码')
      }
    },
    onFailed (error) {
      console.log(error)
      if (error.errors[0]) {
        this.$toast({
          message: error.errors[0].message,
          position: 'top'
        })
      }
    },
    async onSendSms () {
      try {
        await this.$refs['login-form'].validate('mobile')
        this.isSendSmsLoading = true
        await sendSms(this.user.mobile)
        this.isCountDownShow = true
      } catch (err) {
        let message = ''
        if (err && err.response && err.response.status === 429) {
          message = '发送太频繁了，请稍后重试'
        } else if (err.name === 'mobile') {
          message = err.message
        } else {
          message = '发送失败，请稍后重试'
        }
        this.$toast({
          message: message,
          position: 'top'
        })
      }
      this.isSendSmsLoading = false
    }
  }
}
</script>

<style scoped lang="less">
.login-container {
    .send-btn {
        width: 88px;
        height: 23px;
        background-color: #ededed;
        .van-button__text {
                font-size: 11px;
        }
    }
    .login-btn-wrap {
        padding: 26px 16px;
        .login-btn {
            background-color: #6db4fb;
            border: none;
        }
    }
}
</style>
