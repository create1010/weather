<template>
    <main>
        <div class="container max-w-[1200px] mx-auto">
            <!-- 渲染當日天氣 -->
            <div class="list flex flex-wrap gap-10 justify-center my-5">
                <div class="flex flex-col w-[480px]" v-for="(item, index) in weatherList.values" :key="index">
                    <div
                        class="bg-gradient-to-r from-orange-600 to-orange-300 text-white p-2.5 rounded-t-lg overflow-hidden">
                        <div class="flex justify-between mb-2">
                            <div class="flex items-center">
                                <font-awesome-icon :icon="getWeatherIcon(item)" />
                                <span class="text-lg ml-1.5">{{ item.weatherElement[10].time[0].elementValue[0].value
                                    .split('。')[0] }}</span>
                            </div>
                            <div class="text-2xl relative z-30 circle-in">現在時間：{{ formateTime }}</div>
                        </div>
                        <div class="flex justify-between items-center mb-2.5">
                            <div class="text-5xl font-bold">{{ item.weatherElement[1].time[0].elementValue[0].value }}°
                            </div>
                            <div class="relative z-20 circle-out">降雨機率： {{
                                item.weatherElement[0].time[0].elementValue[0].value }}%</div>
                        </div>
                        <div class="flex justify-between items-center">
                            <div class="flex items-center">
                                <div class="mr-1.5">{{ item.weatherElement[12].time[0].elementValue[0].value }}°</div>
                                /
                                <div class="ml-1.5">{{ item.weatherElement[8].time[0].elementValue[0].value }}</div>
                            </div>
                            <div class="relative z-10 circle-out-big">{{ item.locationName }}</div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- 渲染其他天氣 -->
            <div class="otherday bg-slate-500 flex justify-evenly items-center py-2.5 rounded-lg my-5">
                <div class="day text-stone-300 flex justify-center items-center cursor-pointer hover:text-stone-800 transition duration-500"
                    v-for="item in otherDays" :key="item.id">
                    <span>{{ item.text }}</span>
                    <font-awesome-icon class="ml-1.5" :icon="item.icons" />
                </div>
            </div>
        </div>
    </main>
</template>

<script setup>
import { reactive, onMounted, computed } from 'vue';
//初始化陣列
const weatherList = reactive({ values: [] });
//取得現在時間
const now = new Date();
const hours = now.getHours();
const minutes = now.getMinutes();
const formateTime = `${hours}:${minutes < 10 ? '0' : ''}${minutes}`;

// 其他天資訊
const otherDays = reactive([
    { id: '2', text: '星期二', icons: ['far', 'sun'], city: 'Taipei' },
    { id: '3', text: '星期三', icons: ['fas', 'cloud-showers-heavy'], city: 'Taipei' },
    { id: '4', text: '星期四', icons: ['fas', 'cloud-showers-heavy'], city: 'Taipei' },
    { id: '5', text: '星期五', icons: ['fas', 'cloud-sun'], city: 'Taipei' }
]);

// 當日資訊
// const todayText = reactive([
//     { id: '1', text: '晴天', icons: ['far', 'sun'], city: 'Taipei' },
//     { id: '2', text: '晴天', icons: ['far', 'sun'], city: 'Taipei' },
//     { id: '3', text: '雨天', icons: ['fas', 'cloud-showers-heavy'], city: 'Taipei' },
//     { id: '4', text: '雨天', icons: ['fas', 'cloud-showers-heavy'], city: 'Taipei' },
//     { id: '5', text: '陰天', icons: ['fas', 'cloud-sun'], city: 'Taipei' },
//     { id: '6', text: '陰天', icons: ['fas', 'cloud-sun'], city: 'Taipei' },
//     { id: '7', text: '晴天', icons: ['far', 'sun'], city: 'Taipei' },
//     { id: '8', text: '晴天', icons: ['far', 'sun'], city: 'Taipei' },
//     { id: '9', text: '晴天', icons: ['far', 'sun'], city: 'Taipei' },
//     { id: '10', text: '晴天', icons: ['far', 'sun'], city: 'Taipei' }
// ]);
//依照天氣描述顯示不同icon，暫時例外條件return 下雨的icon，因尚未看完其他天氣描述
const getWeatherIcon = (e) => {
    const description = e.weatherElement[10].time[0].elementValue[0].value.split('。')[0];
    if (description.includes('晴時多雲')) {
        return ['far', 'sun']
    }
    return ['fas', 'cloud-sun']
}

//fetch
onMounted(async () => {
    try {
        const response = await fetch('https://opendata.cwa.gov.tw/api/v1/rest/datastore/F-D0047-063?Authorization=CWA-5FBF388C-7076-4D4E-BBA3-E3A3BDE038DA&format=JSON&elementName=&sort&elementName=time');
        const data = await response.json();
        weatherList.values = data.records.locations[0].location;
        console.log(weatherList.values);

    } catch (error) {
        console.log('錯誤', error);
    }
})
</script>

<style scoped>
.circle-in::before {
    @apply content-[''] w-24 h-24 bg-yellow-400 absolute -top-14 -right-7 rounded-full -z-10 opacity-80;
}

.circle-out::before {
    @apply content-[''] w-44 h-44 bg-orange-400 absolute -top-32 -right-14 rounded-full -z-10 opacity-80;
}

.circle-out-big::before {
    @apply content-[''] w-60 h-60 bg-rose-400 absolute -top-44 -right-20 rounded-full -z-10 opacity-80;
}
</style>