
<template>
  <div class="seats-container">
    <!-- 舞台头部部分 -->
    <div class="header-container">
      <div class="seats-header-info">
        <div class="stage">
        舞台</div>
        <div class="seat-info">您的位子：
            <span>{{ userInfo.deskNumber }}桌</span>
        </div>
        <div class="gate-stage">
          <div class="gate"></div>
          <span class="gate-text">入口</span>
        </div>
      </div>
    </div>
    <div class="desk-position-wrap">
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
        <!-- 座位 -->
        <div class="dinner-seats">
          <div
            v-for="(seat, index) in dinnerSeat"
            :key="index">
            <div
              class="dinner-seat-off"
              v-if="seat != userInfo.deskNumber"></div>
            <div
              class="dinner-seat-on"
              v-else>
                <div class="desk-number">{{ seat }}</div>
              </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'

export default {
  name: 'seats',
  data () {
    return {
      // 晚宴座位信息
      dinnerSeat: [
        '8', '6', '5', '3', '2', '1', '15', '13', '12', '11', '10', '9',
        '22', '21', '20', '19', '18', '16', '30', '29', '28', '26', '25', '23',
        '38', '36', '35', '33', '32', '31', '45', '43', '42', '41', '40', '39',
        '52', '51', '50', '49', '48', '46', '60', '59', '58', '56', '55', '53',
        '68', '66', '65', '63', '62', '61', '75', '73', '72', '71', '70', '69'
      ]
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
  created () {
    this.$_appHeader({
      title: '晚宴座位',
      leftOptions: {
        preventGoBack: true
      },
      buttonTab: {
        show: true,
        selected: 'dinner',
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
  }
}
</script>

<style lang="less" scoped>
@import '../../../../assets/css/theme.less';

.seats-container {
  overflow: hidden;
  background-color: @color__white;
}

.header-container {
  position: relative;
}

.seats-header-info {
  z-index: 99999;
  position: absolute;
  width: 100%;
  height: .776rem;
  background-color: @color__white;
}

.seats-header-info .stage,
.seats-header-info .seat-info,
.desk-number,
.dinner-seat-on {
  display: flex;
  display: -webkit-flex;
  justify-content: center;
  -webkit-justify-content: center;
  align-items: center;
  -webkit-align-items: center;
}

.seats-header-info .stage {
  width: 3rem;
  height: .36rem;
  margin: 0 auto;
  background-image: url(../assets/images/stage.png);
  background-size: 3rem .36rem;
  font-size: 12px;
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

.seats-header-info .gate-stage {
  position: absolute;
  left: 0;
  top: .2rem;
}

.seating-chart {
  margin-top: .15rem;
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

.desk-position-wrap {
  position: relative;
  width: 100%;
  margin-top: .776rem;
  height: 85%;
  overflow: scroll;
}

.seating-chart .gate-one,
.seating-chart .gate-two,
.seating-chart .gate-three,
.seating-chart .gate-four,
.seating-chart .gate-five {
  position: absolute;
  left: 0;
  color: @color__green;
}

.seating-chart .gate-one {
  top: 1rem;
}

.seating-chart .gate-two {
  top: 1.45rem;
}

.seating-chart .gate-three {
  top: 3.3rem;
}

.seating-chart .gate-four {
  top: 3.75rem;
}

.seating-chart .gate-five {
  top: 5rem;
}

.dinner-seats {
  width: 3rem;
  display: flex;
  display: -webkit-flex;
  flex-wrap: wrap;
  -webkit-flex-wrap: wrap;
  justify-content: space-between;
  -webkit-justify-content: space-between;
  margin: 0 auto;
}

.dinner-seat-off,
.dinner-seat-on {
  height: .36rem;
  width: .36rem;
  margin: 0 .05rem .18rem;
  background-image: url(../assets/svgs/dinner_desk_off.svg);
  background-size: 100% 100%;
}

.dinner-seat-on {
  background-image: url(../assets/svgs/dinner_desk_on.svg);
}

.desk-number {
  width: .3rem;
  height: .3rem;
  border-radius: 50%;
  background: @color__blue;
  color: @color__white;
}
</style>
