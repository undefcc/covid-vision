<template>
  <div id="china" class="box-center"></div>
</template>
  
<script setup lang='ts'>
import { AreaTree, Children, Diseaseh5Shelf } from '~~/types'
import chinaMap from "../assets/china.json"
import { geoCoordMap } from '../assets/geoMap'

const { appContext } = getCurrentInstance()
const echarts = appContext.config.globalProperties.$echarts
echarts.registerMap("china", chinaMap)

const props = defineProps<{
  chinaDetail: Diseaseh5Shelf
}>()

const emit = defineEmits<{
  (e: 'change', id: Children): void
}>()

onMounted(() => {
  initCharts()
})

const initCharts = () => {
  const city = props.chinaDetail?.areaTree?.[0]?.children || []
  let FirstCityArea = { name: '上海', value: geoCoordMap['上海'].concat(10000) } // city[9]
  emit("change", FirstCityArea)
  const data = city.map(v => {
    return {
      name: v.name,
      value: geoCoordMap[v.name].concat(v.total.nowConfirm),
      children: v.children
    }
  })
  // data[9]['selected'] = true
  const charts = echarts.init(document.querySelector('#china') as HTMLElement)
  charts.setOption({
    geo: {
      map: "china",
      aspectScale: 0.8,
      layoutCenter: ["50%", "50%"],
      layoutSize: "100%",
      itemStyle: {
        //  normal: {
        areaColor: {
          type: "linear-gradient",
          x: 0,
          y: 1200,
          x2: 1000,
          y2: 0,
          colorStops: [
            {
              offset: 0,
              color: "#152E6E", // 0% 处的颜色
            },
            {
              offset: 1,
              color: "#0673AD", // 50% 处的颜色
            },
          ],
          global: true, // 缺省为 false
        },
        shadowColor: "#0f5d9d",
        shadowOffsetX: 0,
        shadowOffsetY: 15,
        opacity: 0.5,
        // },

      },
      emphasis: {
        areaColor: "#0f5d9d",
      },
      regions: [
        {
          name: "南海诸岛",
          itemStyle: {
            areaColor: "rgba(0, 10, 52, 1)",
            borderColor: "rgba(0, 10, 52, 1)",
            //normal: {
            opacity: 0,
            label: {
              show: false,
              color: "#009cc9",
            },
            //},
          },
          label: {
            show: false,
            color: "#FFFFFF",
            fontSize: 12,
          },
        },
      ],
    },
    series: [
      {
        type: "map",
        map: "china",
        aspectScale: 0.8,
        layoutCenter: ["50%", "50%"], //地图位置
        layoutSize: "100%",
        label: {
          show: true,
          color: "#FFFFFF",
          fontSize: 12,
        },
        itemStyle: {
          areaColor: "#0c3653",
          borderColor: "#1cccff",
          borderWidth: 1.8,
        },
        emphasis: {
          areaColor: "#56b1da",
          label: {
            show: true,
            color: "#fff",
          },
        },
        data: data,
      },
      {
        type: 'scatter',
        coordinateSystem: 'geo',
        symbol: 'pin',
        symbolSize: [45, 45],
        label: {
          show: true,
          color: "#fff",
          formatter(value: any) {
            return value.data.value[2]
          }
        },
        itemStyle: {
          color: '#1E90FF', //标志颜色
        },
        data: data,
      },
    ],
  })
  charts.on('click', (e: any) => {
    FirstCityArea = e.data
    emit("change", FirstCityArea)
  })
}

</script>
  
<style scoped>
.box-center {
  flex: 1;
  height: 100%;
}
</style>