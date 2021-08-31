<template>
    <div class="login-container">
      <van-cell-group v-if="user" class="my-info">
        <van-cell
          title="单元格"
          value="内容"
          center
          :border=false
          class="base-info"
          >
            <van-image
              class="avatar"
              round
              slot="icon"
              fit="cover"
              :src="currentUser.photo"
            />
              <div slot="title" class="name">{{currentUser.name}}</div>
              <van-button
                size="small"
                round
                class="update-btn"
              >
                编辑资料
              </van-button>
          </van-cell>
        <!-- <van-grid :border=false class="data-info">
          <van-grid-item class="data-info-item">
              <div class="text-warp" slot="text">
                  <div class="count">{{currentUser.art_count}}</div>
                  <div class="text">头条</div>
              </div>
          </van-grid-item>
          <van-grid-item class="data-info-item">
              <div class="text-warp" slot="text">
                  <div class="count">{{currentUser.follow_count}}</div>
                  <div class="text">关注</div>
              </div>
          </van-grid-item>
          <van-grid-item class="data-info-item">
              <div class="text-warp" slot="text">
                  <div class="count">{{currentUser.fans_count}}</div>
                  <div class="text">粉丝</div>
              </div>
          </van-grid-item>
          <van-grid-item class="data-info-item">
              <div class="text-warp" slot="text">
                  <div class="count">{{currentUser.like_count}}</div>
                  <div class="text">获赞</div>
              </div>
          </van-grid-item>
        </van-grid> -->
        <div class="data-info">
          <table>
            <tr>
              <td>
                <div class="text-warp" slot="text">
                  <div class="count">{{currentUser.art_count}}</div>
                  <div class="text">头条</div>
              </div>
              </td>
              <td>
                <div class="text-warp" slot="text">
                  <div class="count">{{currentUser.follow_count}}</div>
                  <div class="text">关注</div>
              </div>
              </td>
              <td>
                <div class="text-warp" slot="text">
                  <div class="count">{{currentUser.fans_count}}</div>
                  <div class="text">粉丝</div>
              </div>
              </td>
              <td>
                <div class="text-warp" slot="text">
                  <div class="count">{{currentUser.like_count}}</div>
                  <div class="text">获赞</div>
              </div>
              </td>
            </tr>
          </table>
        </div>
      </van-cell-group>
      <div v-else class="not-login">
        <div @click="$router.push('/login')">
          <img class="mobile" src="../../assets/my/not-login.png" alt="" />
        </div>
        <div class="text">登陆/注册</div>
      </div>
      <van-grid :column-num="2" class="nva-grid">
        <van-grid-item class="nva-grid-item" icon="star-o" text="收藏" />
        <van-grid-item class="nva-grid-item" icon="browsing-history-o" text="历史" />
      </van-grid>
      <van-cell style="margin-top: 4px;" title="消息通知" is-link to="" />
      <van-cell title="小智同学" is-link to="" />
      <!-- <van-button v-if="user" class="login-btn" block style="margin-top:5px">退出登陆</van-button> -->
      <button v-if="user" class="loginout-btn" @click="onLoginout">退出登陆</button>
    </div>
</template>

<script>
import { mapState } from 'vuex'
import { getCurrentUser } from '@/api/user'
// import { Dialog } from 'vant'

export default {
  name: 'myIndex',
  data () {
    return {
      currentUser: {}
    }
  },
  created () {
    this.loadcurrentUser()
  },
  computed: {
    ...mapState(['user'])
  },
  methods: {
    async loadcurrentUser () {
      const { data } = await getCurrentUser()
      this.currentUser = data.data
    },
    onLoginout () {
      this.$dialog.confirm({
        title: '退出提示',
        message: '确认要退出吗？'
      })
        .then(() => {
        // on confirm
          this.$store.commit('setUser', null)
        })
        .catch(() => {
        // on cancel
        })
    }
  }
}
</script>

<style lang="less">
.login-container {
  .my-info {
      background-color: pink;
      .base-info{
          box-sizing: border-box;
          widows: 115px;
          background-color: unset;
          padding-top: 38px;
          padding-bottom: 11px;
          .avatar {
              box-sizing: border-box;
              width: 66px;
              height: 66px;
              border: 1px solid #fff;
              margin-right: 11px;
          }
          .name {
              font-size: 15px;
              color: rgba(61, 64, 66, 0.884);
          }
          .update-btn {
              height: 22px;
              font-size: 10px;
              color: rgba(61, 64, 66, 0.884);
          }
        }
        // .data-info {
        //     .data-info-item {
        //         height: 65px;
        //          color: rgba(61, 64, 66, 0.884);
        //          .text-warp {
        //              display: flex;
        //              flex-direction: column;
        //              justify-content: center;
        //              align-items: center;
        //             .count {
        //                 font-size: 18px;
        //             }
        //             .text {
        //                 font-size: 11px;
        //             }
        //             ::v-deep .van-grid-item__content {
        //                background-color: unset;
        //             }
        //          }
        //       }
        //   }
        .data-info {
          table {
            width: 100%;
            tr{
              height: 65px;
              td {
                .text-warp {
                    display: flex;
                    flex-direction: column;
                    justify-content: center;
                    align-items: center;
                    .count {
                        font-size: 18px;
                    }
                    .text {
                         font-size: 11px;
                     }
                }
              }
            }
          }
        }
      }
  }
  .not-login {
    height: 180px;
    background-color: pink;
    background-size: cover;
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    .mobile {
      border-radius: 50%;
      width: 66px;
      height: 66px;
    }
    .text {
      color: #fff;
      font-size: 14px;
    }
  }
   .loginout-btn {
     color: red;
     width: 100%;
     height: 38px;
     margin-top: 4px;
     background-color: #fff;
     border: none;
     font-size: 10px;
  }
  /deep/ .nav-grid {
      .nav-grid-item {
          height: 70px;
          .star-o {
              color: #eb5253;
          }
      }
  }
</style>
