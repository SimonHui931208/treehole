<template>
  <div class="home">
    <div class="content-box">
      <img 
        src="./assets/bg.png" 
        alt="背景图" 
        class="bg-img" 
        :style="{width: `${bgWidth}px`, height: `${bgHeight}px`}"
      >
      <img 
        src="./assets/left-door.png"
        alt="左门"
        class="left-door" 
        :class="{'left-door-ani' : isCloseDoor}"
        :style="{width: `${doorWidth}px`, left: `${leftDoorToLeft}px`}"
      >
      <img 
        src="./assets/right-door.png" 
        alt="右门" 
        class="right-door" 
        :class="{'right-door-ani' : isCloseDoor}"
        :style="{width: `${doorWidth}px`, left: `${rightDoorToLeft}px`}"
      >
      <div 
        v-if="showPillType === 'half'"
        class="paper-pos" 
        :class="{'drop-paper': isDropIntoPill}"
        :style="{left: `${paperToLeft}px`, bottom: `${paperToBottom}px`, width: `${paperWidth}px`, height: `${paperHeight}px`}" >
        <paper @action="doAction"/>
      </div>
      <img 
        src="./assets/pill-bottom.png" 
        alt="胶囊下方" 
        class="pill-bottom"
        :style="{left: `${pillBottomToLeft}px`, width: `${pillWidth}px`}"
        v-if="showPillType === 'half'"
      >
      <img 
        src="./assets/pill-top.png" 
        alt="胶囊上方" 
        class="pill-top"
        :class="{'pill-top-rotate': pillTopToBottom !== 0 }"
        :style="{left: `${pillTopToLeft}px`, bottom: `${pillTopToBottom}px`, width: `${pillWidth}px`}"
        v-if="showPillType === 'half'"
      >
      <img 
        src="./assets/pill.png" 
        alt="胶囊" 
        class="pill-whole"
        :style="{left: `${pillBottomToLeft}px`, width: `${pillWidth}px`, bottom: `${pillBottomToBottom}px`}"
        v-if="showPillType === 'whole'"
      >
    </div>
    <response v-if="isShowResponse" @action="reset"/>
  </div>
</template>

<script>
import Paper from './components/Paper'
import Response from './components/Response'

const bgHeight = document.documentElement.clientHeight
const bgWidth = bgHeight * 1.5
const doorWidth = bgWidth * 0.12
const leftDoorToLeft = bgWidth * 0.53
const rightDoorToLeft = bgWidth * 0.65
const paperToLeft = bgWidth * 0.22
const paperToBottom = bgHeight * 0.4
const paperWidth = bgWidth*0.25
const paperHeight = paperWidth*1.3

const pillBottomToLeft = bgWidth * 0.32
const pillBottomToBottom = 0

const pillTopToLeft = bgWidth * 0.16
const pillTopToBottom = 0
const pillWidth = bgWidth * 0.05

export default {
  name: 'Index',
  components: {
    Paper,
    Response
  },
  data() {
    return {
      // 是否关门
      isCloseDoor: false,
      isDropIntoPill: false,
      isShowResponse: false,
      // 展示的胶囊类型
      showPillType: 'half',
      bgWidth,
      bgHeight,
      doorWidth,
      leftDoorToLeft,
      rightDoorToLeft,
      paperToLeft,
      paperToBottom,
      paperWidth,
      paperHeight,
      pillBottomToLeft,
      pillBottomToBottom,
      pillTopToLeft,
      pillTopToBottom,
      pillWidth
    }
  },
  methods: {
    doAction() {
      // 卷纸动画
      this.paperHeight *= 0.12
      setTimeout(() => {
        // 纸落入胶囊中
        this.isDropIntoPill = true
        setTimeout(() => {
          // 盖上胶囊盖
          this.pillTopToLeft = bgWidth * 0.32
          this.pillTopToBottom = bgHeight * 0.088
          setTimeout(() => {
            // 替换胶囊图片
            this.showPillType = 'whole'
            // 胶囊移动到门里
            this.pillBottomToLeft *= 1.9
            this.pillBottomToBottom = bgHeight * 0.06
            // 关门动画
            setTimeout(() => {
              this.showPillType = ''
              this.isCloseDoor = true
              // 打开响应弹窗
              setTimeout(() => this.isShowResponse = true, 2000)
            }, 1000)
          }, 1300)
        }, 1500)
      }, 800)
    },
    reset() {
        // 是否关门
      this.isCloseDoor =  false
      this.isDropIntoPill = false
      this.isShowResponse = false
        // 展示的胶囊类型
      this.showPillType = 'half'
      
      this.paperToLeft = paperToLeft
      this.paperToBottom = paperToBottom
      this.paperHeight = paperHeight
      this.pillBottomToLeft = pillBottomToLeft
      this.pillBottomToBottom = pillBottomToBottom
      this.pillTopToLeft = pillTopToLeft
      this.pillTopToBottom = pillTopToBottom
    }
  }
}
</script>

<style>
  .home{
    position: absolute;
    text-align: center;
    background: linear-gradient(rgba(0, 0, 0, .4), rgba(0, 0, 0, 1));
    width: 100%;
    height: 100%;
    overflow: hidden;
  }

  .content-box{
    display: inline-block;
    position: relative;
    height: 100%;
  }

  .bg-img{
    width: auto;
    height: 100%;
  }

  .left-door, .right-door{
    position: absolute;
    bottom: 0;
  }
  .left-door{
    transform-origin: left;
    transform: rotateY(-50deg);
  }
  .left-door-ani{
    animation: left-door-close 2s ease-out forwards;
  }
  .right-door{
    transform-origin: right;
    transform: rotateY(50deg);
  }
  .right-door-ani{
    animation: right-door-close 2s ease-out forwards;
  }
  /* 纸的动画 */
  .paper-pos{
    position: absolute;
    overflow: hidden;
    /* 卷纸动画500ms */
    transition: height 500ms linear;
    z-index: 11;
  }
  .drop-paper{
    animation: paper-fly-in-pill 1.2s ease-in-out forwards;
  }
  /* 胶囊动画及样式 */
  .pill-bottom, .pill-top{
    position: absolute;
    bottom: 0;
    z-index: 12;
  }
  .pill-top{
    transform: rotateZ(-120deg);
    transition: left 600ms ease-in, bottom 600ms linear;
  }
  .pill-top-rotate {
    animation: rotate-pill-top 600ms ease-in forwards;
  }
  .pill-whole{
    position: absolute;
    transform-origin: center;
    transition: left 1s ease-out, bottom 1s ease-out;
    animation: rotate-pill 1s ease-in forwards;
  }

  /* 关门动画 */
  @keyframes left-door-close {
    0%{
      transform: rotateY(-50deg);
    }
    100%{
      transform: rotateY(0deg);
    }
  }
  @keyframes right-door-close {
    0%{
      transform: rotateY(50deg);
    }
    100%{
      transform: rotateY(0deg);
    }
  }

  /* 卷纸动画 */
  @keyframes paper-fly-in-pill {
    0%{
      transform: rotateZ(90deg) scale(.8);
    }
    100%{
      transform: rotateZ(90deg) scale(.3);
      bottom: 30px;
    }
  }
  /* 旋转胶囊-上方 */
  @keyframes rotate-pill-top {
    0%{
      transform: rotateZ(-120deg);
    }
    100%{
      transform: rotateZ(0deg);
    }
  }
  /* 旋转胶囊 */
  @keyframes rotate-pill {
    0%{
      transform: rotateZ(0deg) scale(.8);
    }
    25%{
      transform: rotateZ(180deg) scale(.7);
    }
    50%{
      transform: rotateZ(360deg) scale(.6);
    }
    75%{
      transform: rotateZ(540deg) scale(.5);
    }
    100%{
      transform: rotateZ(720deg) scale(.4);
    }
  }
</style>
