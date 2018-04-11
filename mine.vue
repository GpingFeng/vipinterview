
<template>
  <div class="seats-container">
    <!-- 舞台头部部分 -->
    <div class="header-container">
      <div class="seats-header-info">
        <div class="stage">
        舞台</div>
        <div class="seat-info">您的位子：
          <img
            src="../assets/svgs/seat_on.svg"
            srcset="../assets/svgs/seat_on.svg 2x"
            alt="选中的座位">
            <span>{{ userInfo.seatNumber[0] + '排' + userInfo.seatNumber.substring(1) + '座' }}</span>
        </div>
        <div class="gate-stage">
          <div class="gate"></div>
          <span class="gate-text">入口</span>
        </div>
      </div>
    </div>
    <div class="seats-position-wrap">
      <!-- 座位表侧边导航栏 -->
      <div class="seats-sidebar">
        <span
          class="sidebar-navigation-seat"
          v-for="rownum in rowsNumber"
          :key="rownum">
        {{ rownum }}
        </span>
      </div>
      <!-- 座位表 -->
      <div class="seating-chart">
        <!-- 五个入口 -->
        <div class="gate-one">
          <div class="gate"></div>
          <span class="gate-text">入口</span>
        </div>
        <div class="gate-two">
          <div class="gate"></div>
          <span class="gate-text">入口</span>
        </div>
        <div class="gate-three">
          <div class="gate"></div>
          <span class="gate-text">入口</span>
        </div>
        <div class="gate-four">
          <div class="gate"></div>
          <span class="gate-text">入口</span>
        </div>
        <div class="gate-five">
          <div class="gate"></div>
          <span class="gate-text">入口</span>
        </div>
        <!-- 上半部分的座位 -->
        <section class="first-floor">
          <div
            class="seating-chart-rows"
            v-for="row in firstPartRowNumber"
            :key="row">
            <div
              class="seat"
              v-for="col in firstRowSeatNumber"
              :key="col"
              :id="`${ rowsNumber[row-1] + col }`">
            </div>
          </div>
        </section>
        <!-- 下半部分的座位 -->
        <section class="second-floor">
          <div
            class="seating-chart-rows"
            v-for="row in secondPartRowNumber"
            :key="row">
            <div
              class="seat"
              v-for="col in secondRowSeatNumber"
              :key="col"
              :id="`${ rowsNumber[row-1+firstPartRowNumber] + col }`">
            </div>
          </div>
        </section>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import throttle from 'lodash/throttle'

export default {
  name: 'seats',
  data () {
    return {
      rowsNumber: [                         // 侧边导航栏
        'A', 'B', 'C', 'D', 'E', 'F', 'G',
        'H', 'I', 'J', 'K', 'L', 'M', 'N',
        'O', 'P', 'Q', 'R', 'S', 'T', 'U',
        'V', 'W', 'X', 'Y'
      ],
      firstPartRowNumber: 18,               // 上半部分座位行数
      secondPartRowNumber: 7,               // 下半部分座位行数
      firstRowSeatNumber: 30,               // 上半部分座位每行的座位数，注意第一个是空值
      secondRowSeatNumber: 32               // 下半部分每行的座位数
    }
  },
  computed: {
    ...mapState([
      'userInfo'
    ]),
    ...mapState('app', [
      'appHeaderCount'
    ])
  },
  watch: {
    'appHeaderCount.back' () {
      this.$router.replace({ name: 'user' })
    }
  },
  methods: {
    /**
    * @desc 滚动座位时候的操作
    */
    scrollSeats () {
      var seatingContainer = document.getElementsByClassName('seats-position-wrap')[0]
      // 使用的是loadash中的throttle
      seatingContainer.addEventListener('scroll', throttle(() => {
        var seatingContainer = document.getElementsByClassName('seats-position-wrap')[0]
        // 计算出座位表的滑动的距离，使用的是scrollTop
        var yScroll = seatingContainer.scrollTop
        var sidebar = document.getElementsByClassName('seats-sidebar')[0]
        // 使用styled方法直接给侧边栏添加一个transform属性
        sidebar.style.transform = 'translate(' + '0px, ' + '-' + yScroll + 'px)'
        // 兼容IOS
        sidebar.style.webkitTransform = 'translate(' + '0px, ' + '-' + yScroll + 'px)'
      }, 100), false)
    }
  },
  created () {
    this.$_appHeader({
      title: '我的座位',
      leftOptions: {
        preventGoBack: true
      },
      buttonTab: {
        show: true,
        selected: 'seats',
        options: [{
          title: '会议',
          key: 'seats',
          route: {
            name: 'seats'
          }
        }, {
          title: '晚宴',
          key: 'dinner',
          route: {
            name: 'dinner'
          }
        }]
      }
    })
    this.$_appTabbar({
      show: false
    })
  },
  mounted () {
    this.scrollSeats()
    this.$nextTick(() => {
      var targetSeatId = this.userInfo.seatNumber
      var targetSeatDom = document.getElementById(targetSeatId)
      targetSeatDom.classList.add('target')
    })
  }
}
</script>

<style lang="less" scoped>
@import '../../../../assets/css/theme.less';

.seats-position-wrap {
  position: relative;
  margin-top: .776rem;
  width: 100%;
  height: 85%;
  overflow: scroll;
}

.header-container {
  position: relative;
}

.seats-header-info {
  z-index: 0;
  position: absolute;
  width: 100%;
  height: .776rem;
  background-color: @color__white;
}

.seats-header-info .stage {
  width: 3rem;
  height: .36rem;
  margin: 0 auto;
  background-image: url(../assets/images/stage.png);
  background-size: 3rem .36rem ;
  font-size: 12px;
}

.seats-header-info .stage,
.seats-header-info .seat-info,
.sidebar-navigation-seat {
  display: flex;
  display: -webkit-flex;
  justify-content: center;
  -webkit-justify-content: center;
  align-items: center;
  -webkit-align-items: center;
}

.seats-header-info .seat-info {
  width: 100%;
  margin-top: .1rem;
  font-size: 15px;
}

.seats-header-info .seat-info>span {
  color: @color__blue;
  font-size: 16px;
}

.seats-header-info .seat-info>img {
  width: .2rem;
  margin: 0 .05rem;
}

.seats-header-info .gate-stage {
  position: absolute;
  left: 0;
  top: .2rem;
}

.seats-header-info .gate-text,
.seating-chart .gate-text {
  display: inline-block;
  width: .18rem;
  margin-right: -.05rem;
  font-size: 10px;
  color: @color__green;
  text-align: center;
}

.seats-header-info .gate,
.seating-chart .gate {
  display: inline-block;
  width: .04rem;
  height: .24rem;
  background-color: @color__green;
}

.seats-sidebar {
  z-index: -2;
  display: flex;
  display: -webkit-flex;
  flex-wrap: wrap;
  -webkit-flex-wrap: wrap;
  position: fixed;
  right: 0;
  top: 1.236rem;
  width: .2rem;
  background-color: @color__black;
  color: @color__white;
  opacity: 0.4;
  border-radius: 1rem;
}

.seating-chart {
  z-index: -3;
  position: relative;
  padding-left: .3rem;
  width: 7.35rem;
  margin-bottom: .05rem;
}

.seating-chart .gate-one,
.seating-chart .gate-two,
.seating-chart .gate-three,
.seating-chart .gate-four,
.seating-chart .gate-five {
  position: absolute;
  left: 0rem;
  color: @color__green;
}

.seating-chart .gate-one {
  top: .75rem;
}

.seating-chart .gate-two {
  top: 1.1rem;
}

.seating-chart .gate-three {
  top: 3.55rem;
}

.seating-chart .gate-four {
  top: 3.9rem;
}

.seating-chart .gate-five {
  top: 7.3rem;
}

.seating-chart-rows {
  position: relative;
  height: .31rem;
  width: 7.1rem;
  display: flex;
  display: -webkit-flex;
  align-items: center;
  -webkit-align-items: center;
}

.seat,
.target {
  float: left;
  width: .16rem;
  height: .16rem;
  margin: 0 .02rem;
  text-align: center;
  background-image: url(../assets/svgs/seat_off.svg);
  background-size: .16rem .16rem;
}

.target {
  background-image: url(../assets/svgs/seat_on.svg);
}

.sidebar-navigation-seat {
  height: .31rem;
  flex: 0 0 100%;
  -webkit-flex: 0 0 100%;
}

.second-floor {
  .seat:nth-child(10),
  .seat:nth-child(22) {
    margin-right: .3rem;
  }
}

.first-floor {
  margin-left: .2rem;
  .seat:nth-child(9),
  .seat:nth-child(21) {
    margin-right: .3rem;
  }
  .seat:nth-child(3n+1) {
    position: relative;
    &:before {
      content: '';
      display: block;
      position: absolute;
      left: -.02rem;
      top: -.1rem;
      width: .61rem;
      height: .2rem;
      background-image: url('../assets/svgs/desk.svg');
      background-repeat: no-repeat;
      background-size: .61rem .2rem;
    }
  }
}
</style>
