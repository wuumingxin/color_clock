<template>
  <div class="home" @dblclick="toOption" :style="homeItemStyle.clockStyle">
    <div class="box" :style="colckBox">
      <div
        class="item item1"
        :style="[
          { color: homeItemStyle.fontStyle.background },
          homeItemStyle.timeStyle,
        ]"
      >
        {{ hours }}
      </div>
      <div
        class="item item2"
        :style="[
          { color: homeItemStyle.fontStyle.background },
          homeItemStyle.timeStyle,
        ]"
      >
        {{ minutes }}
      </div>
      <div
        class="item item3"
        :style="[
          { color: homeItemStyle.fontStyle.background },
          homeItemStyle.timeStyle,
        ]"
      >
        {{ seconds }}
      </div>
    </div>
    <div class="message">双击打开设置页面</div>
    <router-view />
  </div>
</template>

<script>

export default {
  name: 'Home',
  data () {
    return {
      dateTime: '',
      hours: '12',
      minutes: '30',
      seconds: '00',
      homeItemStyle: {
        clockStyle: {
          background: '#A5DD27'
        },
        timeStyle: {
          background: '#5E24C4'
        },
        fontStyle: {
          background: '#FFF043'
        }
      }
    }
  },
  components: {

  },
  created () {
    this.getTime()
    const ls = localStorage.getItem('itemStyle')
    if (ls !== null) {
      this.homeItemStyle = JSON.parse(ls)
    }
  },
  methods: {
    getTime () {
      const intTime = setInterval(() => {
        const date = new Date()
        // console.log(date)
        this.hours = date.getHours() < 10 ? '0' + date.getHours() : date.getHours()
        this.minutes = date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes()
        this.seconds = date.getSeconds() < 10 ? '0' + date.getSeconds() : date.getSeconds()
      }, 1000)
      return intTime
    },
    toOption () {
      this.$router.push('option')
    }
  },
  destroyed () {
    clearInterval(this.getTime())
  },
  computed: {
    colckBox () {
      const obj = JSON.parse(JSON.stringify(this.homeItemStyle.timeStyle))
      obj.background = obj.background + '80'
      return obj
    }
  }
}
</script>

<style lang="less" scoped>
.home {
  height: 100vh;
  background-color: #a5dd27;
  display: flex;
  justify-content: center;
  align-items: center;
  .message {
    position: absolute;
    bottom: 10px;
    color: #ffffff;
  }
  .box {
    width: 90%;
    background: rgba(94, 36, 196, 0.6);
    backdrop-filter: blur(6px);
    // opacity: 0.3;
    height: 100px;
    border-radius: 20px;
    display: flex;
    justify-content: space-around;
    padding: 15px;
    gap: 30px;
    position: relative;
    .item {
      opacity: 1;
      flex: 1;
      background-color: #5e24c4;
      border-radius: 15px;
      display: flex;
      justify-content: center;
      align-items: center;
      color: yellow;
      font-size: 20px;
      letter-spacing: 10px;
      padding-left: 10px;
    }
    .item1 {
      position: relative;
      &::after {
        content: "pm";
        font-size: 12px;
        position: absolute;
        top: 1px;
        left: 25px;
        letter-spacing: 0;
      }
    }
    &::after {
      content: ":";
      font-size: 30px;
      color: #ffffff;
      line-height: 70px;
      position: absolute;
      left: 33%;
      animation: identifier 3s;
      animation-iteration-count: infinite;
      animation-direction: alternate;
      transform: translateX(-50%);
      top: 10px;
    }
    &::before {
      content: ":";
      font-size: 30px;
      color: #ffffff;
      line-height: 70px;
      position: absolute;
      left: 66%;
      animation: identifier 3s;
      animation-iteration-count: infinite;
      animation-direction: alternate;
      transform: translateX(-50%);
      top: 10px;
    }
    @keyframes identifier {
      from {
        opacity: 1;
      }
      to {
        opacity: 0.3;
      }
    }
  }
}
</style>
