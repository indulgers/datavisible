<template>
  <div class="chartNum">
    <div class="box-item">
      <li :class="{'number-item': !isNaN(item), 'mark-item': isNaN(item) }"
          v-for="(item,index) in orderNum"
          :key="index">
        <span v-if="!isNaN(item)">
         <i ref="numberItem">0123456789</i>
        </span>
        <span class="comma" v-else>{{item}}</span>
      </li>
    </div>
  </div>
</template>
<script>
export default {
  name:"qg_shuju",
  props: {
    value: {
      type: Number, // 具体数值
      default() {
        return 123440;
      },
    },
    time: {
      type: Number, // 滚动要花的时间，单位秒
      default() {
        return 3;
      },
    },
  },
  data() {
    return {
      orderNum: ['0', '0', ',', '0', '0', '0', ',', '0', '0', '0'], // 默认订单总数
    }
  },
  mounted() {
    this.toOrderNum(this.value) // 这里输入数字即可调用
    this.increaseNumber(this.time);
  },
  methods: {
    // 定时增长数字
    increaseNumber (time) {
      let self = this
      this.timer = setInterval(() => {
        self.newNumber = self.newNumber + self.getRandomNumber(1, 100)
        self.setNumberTransform()
      }, time * 1000)
    },
    // 设置文字滚动
    setNumberTransform() {
      const numberItems = this.$refs.numberItem // 拿到数字的ref，计算元素数量
      const numberArr = this.orderNum.filter(item => !isNaN(item))
      // 结合CSS 对数字字符进行滚动,显示订单数量
      for (let index = 0; index < numberItems.length; index++) {
        const elem = numberItems[index]
        elem.style.transform = `translate(-50%, -${numberArr[index] * 10}%)`
      }
    },
    getRandomNumber(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min)
    },
    // 处理传过来的具体值value
    toOrderNum(num) {
      num = num.toString()
      // 把具体值value变成字符串
      if (num.length < 8) {
        num = '0' + num // 如未满八位数，添加"0"补位
        this.toOrderNum(num) // 递归添加"0"补位
      } else if (num.length === 8) {
        // 具体值value中加入逗号
        num = num.slice(0, 2) + ',' + num.slice(2, 5) + ',' + num.slice(5, 8)
        this.orderNum = num.split('') // 将其便变成数据，渲染至滚动数组
      } else {
        // 具体值value数字超过八位显示异常
        this.$message.warning('xxx数量过大，显示异常，请联系后台管理员')
      }
    },
  }
}
</script>

<style scoped>
.box-item {
  position: relative;
  height: 100px;
  font-size: 54px;
  line-height: 41px;
  text-align: center;
  list-style: none;
  color: rgba(236, 200, 152, 1);
  writing-mode: vertical-lr;
  text-orientation: upright;

-moz-user-select: none;
-webkit-user-select: none;
-ms-user-select: none;
-khtml-user-select: none;
user-select: none;
}
.box-item * {
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
}

/* 默认逗号设置 */
.mark-item {
  width: 10px;
  height: 100px;
  margin-right: 5px;
  line-height: 10px;
  font-size: 48px;
  position: relative;
}
.mark-item > span {
  position: absolute;
  width: 100%;
  bottom: 0;
  writing-mode: vertical-rl;
  text-orientation: upright;
}

.number-item {
  width: 41px;
  height: 75px;
  background: #ccc;
  list-style: none;
  margin-right: 5px;
  background:rgba(250,250,250,1);
  border-radius:4px;
  border:1px solid rgba(221,221,221,1);
}
.number-item > span {
  position: relative;
  display: inline-block;
  margin-right: 10px;
  width: 100%;
  height: 100%;
  writing-mode: vertical-rl;
  text-orientation: upright;
  overflow: hidden;
}
.number-item > span > i {
  font-style: normal;
  position: absolute;
  top: 11px;
  left: 50%;
  transform: translate(-50%,0);
  transition: transform 1s ease-in-out;
  letter-spacing: 10px;
}

.number-item:last-child {
  margin-right: 0;
}
</style>
