<template>
  <div class="option">
    <div class="header-btn" @click="goback">
      <img class="img1" src="../assets/image/x.png" alt="" />
    </div>
    <div class="title">定制 色采 时钟</div>
    <div class="clock-box" :style="itemStyle.clockStyle">
      <div class="box" :style="colckBox">
        <div
          class="item item1"
          :style="[
            { color: itemStyle.fontStyle.background },
            itemStyle.timeStyle,
          ]"
        >
          2
        </div>
        <div
          class="item item2"
          :style="[
            { color: itemStyle.fontStyle.background },
            itemStyle.timeStyle,
          ]"
        >
          3
        </div>
        <div
          class="item item3"
          :style="[
            { color: itemStyle.fontStyle.background },
            itemStyle.timeStyle,
          ]"
        >
          1
        </div>
      </div>
    </div>
    <div class="setcolor-box">
      <div class="item">
        <span>时钟背景色</span>
        <div
          class="colorbox"
          @click="setColor(1)"
          :style="itemStyle.clockStyle"
        ></div>
      </div>
      <div class="item">
        <span>时间背景色</span>
        <div
          class="colorbox"
          @click="setColor(2)"
          :style="itemStyle.timeStyle"
        ></div>
      </div>
      <div class="item">
        <span>时间文字色</span>
        <div
          class="colorbox"
          @click="setColor(3)"
          :style="itemStyle.fontStyle"
        ></div>
      </div>
    </div>
    <div class="save-btn" @click="saveColor">应用当前色彩</div>

    <!-- 颜色选择器 -->
    <div
      class="model"
      v-show="isShowColorSeletor"
      @click.self="closeColorSelector"
    >
      <Chrome class="chrome-sc" v-model="colors" />
    </div>
  </div>
</template>

<script>
import { Chrome } from 'vue-color'
export default {
  data () {
    return {
      colors: {
        hex: '#194d33'
      },
      isShowColorSeletor: false,
      itemStyle: {
        clockStyle: {
          background: '#A5DD27'
        },
        timeStyle: {
          background: '#5E24C4'
        },
        fontStyle: {
          background: '#FFF043'
        }
      },
      itemNum: 0
    }
  },
  components: {
    Chrome
  },
  methods: {
    setColor (value) {
      this.isShowColorSeletor = true
      this.itemNum = value
    },
    closeColorSelector () {
      this.isShowColorSeletor = false
      console.log('aaaa')
    },
    goback () {
      history.go(-1)
    },
    saveColor () {
      localStorage.setItem('itemStyle', JSON.stringify(this.itemStyle))
      history.go(-1)
    }

  },
  watch: {
    'colors.hex': {
      handler: function (val, old) {
        switch (this.itemNum) {
          case 1:
            this.itemStyle.clockStyle.background = val
            break
          case 2:
            this.itemStyle.timeStyle.background = val
            break
          case 3:
            this.itemStyle.fontStyle.background = val
            break
          default:
            break
        }
      }
    }
  },
  computed: {
    colckBox () {
      const obj = JSON.parse(JSON.stringify(this.itemStyle.timeStyle))
      obj.background = obj.background + '80'
      return obj
    }
  },
  mounted () {
    const ls = localStorage.getItem('itemStyle')
    if (ls !== null) {
      this.itemStyle = JSON.parse(ls)
    }
  }
}
</script>

<style lang="less" scoped>
.option {
  height: 100vh;
  position: relative;
  z-index: 100;
}
.header-btn {
  height: 50px;
  position: relative;
  img {
    width: 30px;
    height: 30px;
    position: absolute;
    right: 10px;
    top: 50%;
    transform: translateY(-50%);
  }
}
.title {
  font-size: 30px;
  font-weight: 900;
  padding-left: 10px;
}

.clock-box {
  height: 25vh;
  background-color: #a5dd27;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 15px;
  margin: 15px 10px;
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

.setcolor-box {
  display: flex;
  flex-direction: column;
  gap: 5px;
  margin-top: 20px;
  padding: 0 10px;
  .item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    span {
      font-weight: 600;
    }
    .colorbox {
      width: 50px;
      height: 30px;
      background-color: red;
      border-radius: 8px;
    }
  }
}

.model {
  height: 100vh;
  width: 100vw;
  background: rgba(0, 0, 0, 0);
  position: absolute;
  z-index: 999;
  top: 0;
  left: 0;
  .chrome-sc {
    position: absolute;
    bottom: 10px;
    left: 0;
    right: 0;
    margin: auto;
    width: 70%;
  }
}

.save-btn {
  width: 70%;
  height: 40px;
  background-color: #b7b9cc;
  border-radius: 20px;
  text-align: center;
  line-height: 40px;
  position: absolute;
  left: 0;
  right: 0;
  bottom: 20px;
  margin: auto;
  font-weight: 600;
}
</style>
