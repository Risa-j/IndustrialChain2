<template>
  <div class="index" ref="index">
    <dv-loading v-if="loading">Loading...</dv-loading>
    <div v-else class="dashboard">
      <div class="lineOne">
        <dv-decoration-10 class="dv-dec-10"/>
        <div class="middle">
          <dv-decoration-8 class="dv-dec-8"/>
          <div class="middleCenter">
            <dv-decoration-11
                class="dec-11">
              {{ title }}
            </dv-decoration-11>
            <dv-decoration-6
                class="dec-6"
            />
          </div>
          <dv-decoration-8
              class="dv-dec-8"
              :reverse="true"
          />
        </div>
        <dv-decoration-10 class="reverse dv-dec-10"/>
      </div>
      <div class="lineTwo">
        <div class="leftBox">
          <div class="box1 skew bg">
            <div class="rSkew bg">
              <span>数据融合</span>
            </div>
          </div>
          <div class="box2 bg skew" @click="$router.push({name:'Input'})">
            <span class="rSkew">任务分配</span>
          </div>
        </div>
        <div class="rightBox"  @click="$router.push('/copattern/mltipattern')">
          <div class="box1 bg rSkew">
            <span class="skew">协同模式</span>
          </div>
          <div class="box2  rSkew bg ">
            <div class="skew bg">
              <span>
                {{ timeInfo.dateYear }} {{ timeInfo.dateWeek }}
                {{ timeInfo.dateDay }}
              </span>
            </div>
          </div>
        </div>
      </div>
      <div class="main">
        <Main></Main>
      </div>
    </div>
  </div>
</template>

<script setup name="Index">
import {formatTime} from '@/utils/formatTime'
import {ref, reactive, onMounted, onUnmounted} from "vue";
import Main from "@/views/dashboard/components/main/Main.vue";
import elementResizeDetectorMaker from 'element-resize-detector'
import {debounce} from 'lodash'

const index = ref(null);
const title = "多重产业链企业群协同平台"
const WEEK = ['周日', '周一', '周二', '周三', '周四', '周五', '周六']
let height;
let width;

let erd = elementResizeDetectorMaker();
// * 时间内容
const timeInfo = reactive({
  setInterval: 0,
  dateDay: '',
  dateYear: '',
  dateWeek: ''
})

onMounted(() => {
  height= index.value.offsetHeight;
  width = index.value.offsetWidth;
  handleTime();
  erd.listenTo(index.value, debounce((element) => {
    if(width===element.offsetWidth&&height===element.offsetHeight)
    {
      return;
    }
    height= element.offsetHeight;
    width = element.offsetWidth;
    location.reload();
  }, 500));
})
onUnmounted(()=>{
  console.log('unmounted')
  erd.uninstall(index.value);
})
// todo 处理时间监听
const handleTime = () => {
  timeInfo.setInterval = setInterval(() => {
    const date = new Date()
    timeInfo.dateDay = formatTime(date, 'HH: mm: ss')
    timeInfo.dateYear = formatTime(date, 'yyyy-MM-dd')
    timeInfo.dateWeek = WEEK[date.getDay()]
  }, 1000)
}
</script>

<style scoped lang="scss">
.index {
  background-image: url('@/assets/dashboard/pageBg.png');
  height: 100vh;
  padding: 10px;
  overflow: auto;

  .dashboard {
    display: flex;
    flex-direction: column;
    height: 100%;

    .lineOne {
      display: flex;

      .dv-dec-10 {
        width: 25%;
        height: 10px;

        &.reverse {
          transform: rotate(180deg);
        }
      }

      .middle {
        display: flex;
        width: 70%;

        .dv-dec-8 {
          width: 25%;
          height: 50px;
        }

        .middleCenter {
          width: 70%;
          position: relative;
          color: #fff;

          .dec-11 {
            width: 90%;
            height: 60px;
            font-size: 35px;
            position: absolute;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
          }

          .dec-6 {
            height: 10px !important;
            width: 50%;
            position: absolute;
            bottom: -20px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 5;
          }
        }
      }
    }

    .lineTwo {
      display: flex;
      justify-content: space-between;
      overflow: hidden;
      padding: 0 5px;
      color: #fff;
      font-size: 25px;
      height: 4%;

      .leftBox {
        width: 40%;
        height: 100%;
        display: flex;

        .box1 {
          height: 100%;
          width: 50%;
          position: relative;

          div {
            position: absolute;
            height: 100%;
            top: 0;
            left: -10px;
            padding-left: 30px;
          }
        }

        .box2 {
          height: 100%;
          width: 50%;
          margin-left: 10px;
          display: flex;
          justify-content: center;
        }
      }

      .rightBox {
        width: 40%;
        height: 100%;
        display: flex;

        .box1 {
          width: 40%;
          height: 100%;
          margin-right: 10px;

          span {
            display: inline-block;
            margin-left: 50px;
          }
        }

        .box2 {
          width: 60%;
          height: 100%;
          position: relative;

          div {
            position: absolute;
            top: 0;
            right: -10px;
            padding-right: 30px;
            height: 100%;
          }
        }
      }
    }

    .main {
      flex: 1;
      margin-top: 10px;
    }

    .skew {
      transform: skewX(45deg);
    }

    .rSkew {
      transform: skewX(-45deg);
    }

    .bg {
      background-color: #004d8c;
    }
  }
}
</style>

