
<template>
  <div class="user-container">
    <banner></banner>
    <div class="user-info">
      <div class="user-basic-info">
        <img class="user-avatar" :src="userInfo.avatar" @error="handleAvatarImageError">
        <div class="user-main-info">
          <div class="user-name">{{ userInfo.name }}</div>
          <div class="user-department">
            {{ userInfo.departmentName }}&nbsp;|&nbsp;工号：{{ userInfo.employeenumber }}
          </div>
        </div>
      </div>
      <group class="user-number-info" gutter="0">
        <cell-box
          class="user-number-cell"
          is-link>
          <div class="luck-number" v-if="userInfo.signInCode">
              <div class="number">{{ userInfo.signInCode }}</div>
              <div class="number-desc">幸运号码</div>
          </div>
          <div class="luck-number-double" v-else></div>
          <div class="number-line"></div>
          <div
            class="meeting-position"
            @click="meetingPositionClick">
              <div class="number">{{ userInfo.seatNumber }}</div>
              <div class="number-desc">总结会位置</div>
          </div>
          <div
            class="dinner-position"
            @click="dinnerPositionClick">
              <div class="number">{{ userInfo.deskNumber }}</div>
              <div class="number-desc">晚宴桌号</div>
          </div>
        </cell-box>
      </group>
    </div>
    <div class="mine-info">
      <group gutter="0">
        <cell
          class="reward-cell"
          title="我的奖品"
          link="/user/prize"
          is-link>
          <img
            class="cell-icon"
            src="../assets/svgs/user_reward.svg"
            srcset="../assets/svgs/user_reward.svg 2x"
            slot="icon">
        </cell>
      </group>
    </div>
  </div>
</template>

<script>
import { Panel, Group, Cell, CellBox } from 'vux'
import { mapState } from 'vuex'
import Banner from '../../../common/banner'
import defaultAvatar from '../../../../assets/images/default-avatar.png'

export default {
  name: 'user',
  components: {
    Panel,
    Group,
    Cell,
    CellBox,
    Banner
  },
  computed: {
    ...mapState([
      'userInfo'
    ])
  },
  methods: {
    handleAvatarImageError ($event) {
      $event.currentTarget.src = defaultAvatar
    },
    meetingPositionClick (e) {
      this.$router.push({ name: 'seats' })
    },
    dinnerPositionClick () {
      this.$router.push({ name: 'dinner' })
    }
  },
  created () {
    this.$_appHeader({
      title: '我的'
    })
    this.$_appTabbar({
      selected: 'user'
    })
  }
}
</script>

<style lang="less" scoped>
@import 'mx-styles-helper';
@import '../../../../assets/css/theme.less';

@number__desc: #4A4A4A;
@click__active: #ECECEC;

.user-container {
  background-color: @background__color;

  .weui-cell_access.vux-cell-box:after {
    right: .02rem;
  }
}

.user-info {
  padding: 0 .15rem;
  background-color: @color__white;
}

.user-container,
.user-banner,
.banner {
  width: 100%;
}

.user-banner {
  font-size: 0px;
}

.user-basic-info {
  display: flex;
  display: -webkit-flex;
  align-items: center;
  -webkit-align-items: center;
  padding: .15rem 0;
  .user-avatar {
    width: .62rem;
    height: .62rem;
    border-radius: 50%;
  }
  .user-main-info {
    padding-left: .1rem;
  }
}

.user-name {
  font-size: 16px;
  color: @color__black;
  line-height: .22rem;
}

.user-department {
  font-size: 14px;
  color: @font_color__gray;
}

.user-meeting-info {
  display: flex;
  display: -webkit-flex;
  justify-content: center;
  -webkit-justify-content: center;
  align-items: center;
  -webkit-align-items: center;
}

.user-number-cell  {
  padding-left: 0 !important;
  padding-right: 0 !important;
}

.user-number-info .vux-tap-active {
  margin-top: .01rem;
}

.user-number-info .vux-tap-active:active {
  background-color: @color__white;
}

.dinner-position {
  padding-right: .15rem;
}

.meeting-position:active,
.dinner-position:active {
  background-color: @click__active;
}

.luck-number {
  padding-left: .15rem;
  padding-right: .3rem;
  text-align: center;
}

.dinner-position,
.meeting-position {
  flex: 1;
  -webkit-flex: 1;
  text-align: center;
}

.number-line {
  height: .3rem;
  position: relative;

  &:after {
    .setRightLine;
  }
}

.mine-info {
  margin-top: .1rem;

  .cell-icon {
    width: .2rem;
    display: block;
    margin-right: .1rem;
  }
}

.luck-number-double {
  width: .86rem;
  height: .52rem;
  margin-right: .15rem;
  background: url(../assets/images/luck-number-logo.png) center center no-repeat;
  background-size: 75%;
}

.luck-number,
.dinner-position,
.meeting-position {
  padding-top: .13rem;
  padding-bottom: .13rem;
}

.luck-number .number,
.dinner-position .number,
.meeting-position .number {
  font-size: 24px;
}

.luck-number .number {
  color: @color__orange;
}

.dinner-position .number,
.meeting-position .number {
  color: @color__blue;
}

.luck-number .number-desc,
.dinner-position .number-desc,
.meeting-position .number-desc {
  font-size: 14px;
  color: @number__desc;
}

.reward-cell {
  color: @color__black;
  font-size: 16px;
}

#app .weui-cell {
  padding-top: 0;
  padding-bottom: 0;
}

#app .mine-info .weui-cell {
  padding-top: .13rem;
  padding-bottom: .13rem;
}
</style>
