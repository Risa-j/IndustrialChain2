<template>
  <Body :dec-id="3" :icon-name="'icon-tongji4'" :name="'任务通过率'">
  <template #default>
    <div class="chart">
      <Chart/>
    </div>
    <!-- 4个主要的数据 -->
    <div class="main">
      <div
          v-for="(item, index) in numberData"
          :key="index"
          class="item"
      >
        <div class="top">
          <i class="iconfont" :class="[iconFont[index]]"/>
          <dv-digital-flop class="dv-digital-flop" :config="item.config"/>
        </div>
        <p class="text">
          <span> {{ item.text }} </span>
          <span>(件)</span>
        </p>
      </div>
    </div>
  </template>
  </Body>
</template>

<script setup>
import {onMounted, onUnmounted, reactive} from 'vue'
import Chart from './chart/Chart.vue'
import Body from "@/views/dashboard/components/main/component/Body.vue";
// 下层数据
const dataArr = [
  {
    number: 150,
    text: '今日构建总量'
  },
  {
    number: 144,
    text: '总共完成数量'
  },
  {
    number: 361,
    text: '正在编译数量'
  },
  {
    number: 571,
    text: '未通过数量'
  }
]
// 对应图标
const iconFont = [
  'icon-diagnose',
  'icon-monitoring',
  'icon-cloudupload',
  'icon-clouddownload'
]
const numberData = reactive([])
let intervalInstance = null
onMounted(() => {
  setData()
  changeTiming()
})

const setData = () => {
  dataArr.forEach(e => {
    numberData.push({
      config: {
        number: [e.number],
        toFixed: 1,
        content: '{nt}',
        style: {
          fontSize: 24
        }
      },
      text: e.text
    })
  })
}

const changeTiming = () => {
  intervalInstance = setInterval(() => {
    changeNumber()
  }, 2000)
}
const changeNumber = () => {
  numberData.forEach((item, index) => {
    item.config.number[0] += ++index
    item.config = {...item.config}
  })
}
onUnmounted(() => {
  clearInterval(intervalInstance)
})
</script>

<style scoped lang="less">
.chart {
  height: 60%;
  display: flex;
  justify-content: center;
}

.main {
  height: 40%;
  display:grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
  justify-items: center;
  grid-gap: 10px;
  .item{
    .top{
      display:flex;
      i{
        font-size: 20px;
        line-height: 30px;
        color: #004d8c;
      }
    }
    .text{
      span:nth-child(2){
        color: #ee9900;
      }
    }
    .dv-digital-flop {
      height: 30px;
      width: 80px;
    }
  }

}
</style>