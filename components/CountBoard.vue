<template>
  <div class="count-board">
    <div class="number">
      <section>
        <div>今日+ {{ chinaDetail?.chinaAdd.localConfirmH5 || 0 }}</div>
        <div>{{ chinaDetail?.chinaTotal.localConfirm || 0 }}</div>
        <div>本土现有确诊</div>
      </section>
      <section>
        <div>今日+ {{ chinaDetail?.chinaAdd.nowConfirm || 0 }}</div>
        <div>{{ chinaDetail?.chinaTotal.nowConfirm || 0 }}</div>
        <div>现有确诊</div>
      </section>
      <section>
        <div>今日+ {{ chinaDetail?.chinaAdd.confirm || 0 }}</div>
        <div>{{ chinaDetail?.chinaTotal.confirm || 0 }}</div>
        <div>累计确诊</div>
      </section>
      <section>
        <div>今日+ {{ chinaDetail?.chinaAdd.noInfect || 0 }}</div>
        <div>{{ chinaDetail?.chinaTotal.noInfect || 0 }}</div>
        <div>无症状感染者</div>
      </section>
      <section>
        <div>今日+ {{ chinaDetail?.chinaAdd.importedCase || 0 }}</div>
        <div>{{ chinaDetail?.chinaTotal.importedCase || 0 }}</div>
        <div>境外输入</div>
      </section>
      <section>
        <div>今日+ {{ chinaDetail?.chinaAdd.dead || 0 }}</div>
        <div>{{ chinaDetail?.chinaTotal.dead || 0 }}</div>
        <div>累计死亡</div>
      </section>
    </div>
    <div class="left-pie"></div>
    <div class="left-line"></div>
  </div>
</template>
  
<script setup lang='ts'>
import { PropType, getCurrentInstance, onBeforeMount } from 'vue';
import type { Diseaseh5Shelf, StatisGradeCityDetail } from '../types'

const { appContext } = getCurrentInstance()
const echarts = appContext.config.globalProperties.$echarts

const props = defineProps({
  chinaDetail: {
    type: [Object] as PropType<Diseaseh5Shelf>,
  },
  cityDetail: {
    type: [Array] as PropType<Array<StatisGradeCityDetail>>,
  }
})

onMounted(async () => {
  initPie()
  initLine()
})

const initPie = () => {
  const charts = echarts.init(document.querySelector('.left-pie') as HTMLElement)
  charts.setOption({
    backgroundColor: "#223645",
    tooltip: {
      trigger: 'item'
    },
    series: [
      {
        type: 'pie',
        radius: ['40%', '70%'],
        itemStyle: {
          borderRadius: 4,
          borderColor: '#fff',
          borderWidth: 2
        },
        label: {
          show: true,
        },
        emphasis: {
          label: {
            show: true,
            fontSize: '15',
          }
        },
        data: props.cityDetail.map(v => {
          return {
            name: v.city,
            value: v.nowConfirm
          }
        })
      }
    ]
  })
}

const initLine = () => {
  const charts = echarts.init(document.querySelector('.left-line') as HTMLElement)
  charts.setOption({
    backgroundColor: "#223645",
    tooltip: {
      trigger: 'axis'
    },
    xAxis: {
      type: 'category',
      data: props.cityDetail.map(v => v.city),
      axisLine: {
        lineStyle: {
          color: "#fff"
        }
      }
    },
    yAxis: {
      type: 'value',
      axisLine: {
        lineStyle: {
          color: "#fff"
        }
      }
    },
    label: {
      show: true
    },
    series: [
      {
        data: props.cityDetail.map(v => v.nowConfirm),
        type: 'line',
        smooth: true
      }
    ]
  })
}

</script>
  
<style>
body {
  margin: 0;
  padding: 0;
}

.count-board .number {
  color: white;
  background-color: #999;
  display: grid;
  grid-template-columns: auto auto auto;
  grid-template-rows: auto auto;
  margin-bottom: 30px;
}

.count-board .number section {
  background: #223645;
  border: 1px solid #212028;
  padding: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.count-board .number section div:nth-child(2) {
  color: #41b0db;
  padding: 10px 0;
  font-size: 20px;
  font-weight: bold;
}

.left-pie,
.left-line {
  height: 320px;
}
</style>