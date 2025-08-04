<template>
  <NuxtLayout>
    <template #header>
      <div class="title">CovidVision</div>
    </template>
    <template #left-side>
      <CountBoard :chinaDetail="chinaDetail" :cityDetail="cityDetail"></CountBoard>
    </template>
    <template #content>
      <Map :chinaDetail="chinaDetail" @change="handleAreaChange"></Map>
    </template>
    <template #right-side>
      <CountTable :areaDetail="areaDetails"></CountTable>
    </template>
  </NuxtLayout>
</template>

<script setup lang="ts">
import { AreaTree } from '~~/types';
useHead({
  title: 'CovidVision',
  description: 'covid-19 vision',
  keywords: 'covid,map,vision,echarts',
  viewport: 'width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no'
})

// global data
let { diseaseh5Shelf: chinaDetail, statisGradeCityDetail: cityDetail } = useCovData()

// api
const data = ref({
	"ret": 0,
	"info": "",
	"data": {
		"statisGradeCityDetail": [{
			"city": "长宁",
			"nowConfirm": 802,
			"heal": 1574,
			"date": "05/05",
			"sdate": "05/05",
			"syear": 2022,
			"province": "上海",
			"confirmAdd": 5,
			"confirm": 2376,
			"dead": 0,
			"grade": "点击查看详情",
			"mtime": "2022-05-05 08:41:46"
		}, {
			"city": "浦东",
			"heal": 14648,
			"mtime": "2022-05-05 08:44:51",
			"sdate": "05/05",
			"province": "上海",
			"nowConfirm": 2143,
			"confirmAdd": 50,
			"confirm": 16792,
			"dead": 1,
			"grade": "点击查看详情",
			"date": "05/05",
			"syear": 2022
		}]
	}
}) // await useAsyncData('post', () => $fetch(`https://api.inews.qq.com/newsqa/v1/query/inner/publish/modules/list?modules=statisGradeCityDetail,diseaseh5Shelf`))
const res = data.value['data']

// update
chinaDetail = res.diseaseh5Shelf
cityDetail = res.statisGradeCityDetail

// action
let areaDetails = ref<AreaTree>()
const handleAreaChange = (e: AreaTree) => {
  areaDetails.value = e
}
</script>

<style scoped>
.title {
  color: white;
  width: 100%;
  text-align: center;
  font-size: 1.8rem;
}
</style>