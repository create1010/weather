<template>
    <main>
        <div class="container max-w-[1200px] mx-auto">
            <div class="list flex flex-wrap gap-10 justify-center">
                <div class="flex flex-col w-[480px]" v-for="item in days" :key="item.id">
                    <div class="bg-gradient-to-r from-orange-600 to-orange-300 text-white p-5 rounded-t-md">
                        <div class="flex justify-between">
                            <div class="flex items-center ">
                                <font-awesome-icon :icon="item.icons" />
                                <span class="text-lg ml-1.5">{{ item.text }}</span>
                            </div>
                            <div class="text-2xl">23:56</div>
                        </div>
                        <div class="flex justify-between items-center">
                            <div class="text-5xl font-bold">36°</div>
                            <div>日出 08:23</div>
                        </div>
                        <div class="flex justify-between items-center">
                            <div class="flex items-center">
                                <div class="mr-1.5">42°</div>
                                /
                                <div class="ml-1.5">28°</div>
                            </div>
                            <div>台北</div>
                        </div>
                    </div>
                    <div class="otherday bg-amber-800 flex justify-evenly items-center py-5 rounded-b-md">
                        <div class="day text-stone-300 flex justify-center items-center" v-for="item in days"
                            :key="item.id">
                            <span>{{ item.text }}</span>
                            <font-awesome-icon class="ml-1.5" :icon="item.icons" />
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </main>
</template>

<script setup>
import { reactive } from 'vue';

const days = reactive([
    {
        id: '1',
        text: '禮拜一',
        icons: ['far', 'sun']
    },
    {
        id: '2',
        text: '禮拜二',
        icons: ['far', 'sun']
    },
    {
        id: '3',
        text: '禮拜三',
        icons: ['fas', 'cloud-showers-heavy']
    },
    {
        id: '4',
        text: '禮拜四',
        icons: ['fas', 'cloud-showers-heavy']
    },
    {
        id: '5',
        text: '禮拜五',
        icons: ['far', 'sun']
    }
])
</script>

<style scoped></style>