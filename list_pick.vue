<template>
  <div>
    <!--<div>
      选中值为：<span>{{selectedValue}}</span>
    </div>-->
    <div class="box1" :style="{height:liheight*5+'px'}">
      <ul class="list" :style="{top:ulTop+'px'}" @touchstart="touchStart($event)" @touchmove="touchMove($event)" @touchend="touchEnd($event)">
        <li v-for="(item,i) in data" :style="{height:liheight+'px',lineHeight:liheight+'px'}" 
          :class="{'current-pre3':(index-3)===i || (index+3)===i,
            'current-pre2':(index-2)===i || (index+2)===i,
            'current-pre1':(index-1)===i || (index+1)===i,
            'current':index===i}">{{item}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        index: '', //选中下标
        selectedValue: '', //选中值
        oldTop: null,
        top1: null, //从touchstart到touchmove记忆top位置
        ulTop: 0, //用于ul列表定位
      }
    },
    computed: {
      data() { //初始填充最前最后空白
        return [" ", " "].concat(this.dataList).concat([" ", " "])
      }
    },
    watch: { //监听器
      selectedValue(value) { //选择值
        this.$emit('selectedchange', value) //自定义事件，暴露值
      }
    },
    props: {
      dataList: { //暴露数据
        default() {
          return [
            '静安区',
            '长宁区',
            '浦东新区',
            '航头东',
            '嘉定区',
            '黄浦区',
            '宝山区',
            '崇明岛',
            '杨浦区',
            '徐汇区',
          ]
        }
      },
      /*width: {
        default: '100%'
      },*/
      liheight: {
        default: 44
      }
    },
    mounted() {
      this.index = 2;
      this.selectedValue = this.data[this.index];
    },
    methods: {
      touchStart(e) {
        e.stopPropagation();
        e.preventDefault();
        this.oldTop = e.targetTouches[0].pageY;
        this.top1 = this.ulTop;
      },
      touchMove(e) {
        e.stopPropagation();
        e.preventDefault();
        if(event.targetTouches.length == 1) {
          var touch = event.targetTouches[0];
          this.ulTop = touch.pageY - this.oldTop + this.top1;
          var backLength = this.ulTop % this.liheight; //余量
          if(-backLength > this.liheight / 2) {
            this.index = Math.floor(-this.ulTop / this.liheight) + 3; //当前选项的下标
          } else {
            this.index = Math.floor(-this.ulTop / this.liheight) + 2; //当前选项的下标
          }
          this.selectedValue = this.data[this.index];
        }
      },
      touchEnd(e) {
        e.stopPropagation(); //阻止冒泡
        e.preventDefault(); //阻止默认行为
        if(this.ulTop > 0) {
          this.ulTop = 0;
          this.index = 2;
        } else if(-this.ulTop > this.liheight * (this.data.length - 5)) {
          //this.ulTop = -(this.data.length*this.liheight - this.liheight*5);
          this.ulTop = -this.liheight * (this.data.length - 5);
          this.index = this.data.length - 3;
        } else {
          var backLength = this.ulTop % this.liheight; //余量
          if(-backLength > this.liheight / 2) {
            this.index = Math.floor(-this.ulTop / this.liheight) + 3; //当前选项的下标            
            this.ulTop = (this.ulTop - this.liheight - backLength);
          } else {
            this.index = Math.floor(-this.ulTop / this.liheight) + 2; //当前选项的下标
            this.ulTop = (this.ulTop - backLength);
          }
        };
        this.selectedValue = this.data[this.index];
      },
    }
  }
</script>

<style type="text/css" scoped>
  div,
  ul,
  li {
    margin: 0;
    padding: 0;
  }
  
  .box1 {
    overflow: hidden;
    position: relative;
    background: linear-gradient(to bottom, rgb(150, 150, 150), rgb(240, 240, 240), rgb(150, 150, 150));
  }
  
  .list {
    position: absolute;
    top: 0px;
    left: 0;
    width: 100%;
    list-style: none;
    padding: 0;
    text-align: center;
  }
  
  .list li {
    box-sizing: border-box;
    user-select: none;
    transform-origin: center center;
  }
  /*滚动样式*/
  
  .current {
    color: red;
    transform: translateZ(0px) rotateX(0deg);
    /*border-bottom: 1px solid lightskyblue;
    border-top: 1px solid lightskyblue;*/
  }
  
  .current-pre1 {
    color: #555;
    transform: translateZ(-20px) rotateX(30deg);
  }
  
  .current-pre2 {
    color: #888;
    transform: translateZ(-40px) rotateX(60deg);
  }
  
  .current-pre3 {
    color: #888;
    transform: translateZ(-60px) rotateX(70deg);
  }
</style>