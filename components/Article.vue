<template>
    <div class="flex flex-col mx-6 sm:mx-10 lg:mx-0">
        <div class="lg:hidden text-xs uppercase font-semibold tracking-[2px] mb-6"> Articles </div>
        <div class="flex flex-col lg:w-fit group">
            <div class="lg:w-[460px] xl:w-[580px] 2xl:w-[700px] flex flex-col p-6 sm:p-8 mb-4 relative bg-center
                    lg:hover:scale-105 lg:duration-300 lg:group-hover:opacity-50 lg:hover:!opacity-100"
                v-for="(article, index) of articles" :key="index" :style="{ 'background-image': `url(${article.thumbnail})` }">
                <div class="absolute inset-0 bg-gradient-to-r from-bgclr/100 to-bgclr/90"></div>
                <div class="flex flex-col z-10">
                    <div class="text-[11px] tracking-[2px] uppercase"> {{ formatPublishDate(article.pubDate) }} </div>
                    <h1 class="mt-2 font-semibold text-lg leading-tight"> {{ article.title }} </h1>
                    <ul class="mt-4 flex whitespace-nowrap flex-wrap">
                        <li class="text-[10px] bg-topicsbgclr rounded-full font-semibold text-white px-2 py-1 mr-2 mb-2"
                            v-for="category in article.categories">
                            {{ category }}
                        </li>
                    </ul>
                    <div class="mt-2 2xl:mt-1 ml-auto flex items-center"> 
                        <img src="~/assets/icons/medium.svg" height="18" width="18" />
                        <a class="flex" :href="article.link" target="_blank" rel="noopener noreferrer">
                            <span class="mx-2 text-sm"> Medium </span>
                            <img src="~/assets/icons/external.svg" height="12" width="12"/>
                        </a>
                    </div>
                </div>
            </div>
        </div>
        <div class="w-fit mt-2">
            <a class="flex items-center group" :href="props.platforms && props.platforms[2].url" target="_blank" rel="noopener noreferrer">
                <span class="font-semibold tracking-wide text-sm"> View Medium Profile </span>
                <img class="ml-2 group-hover:ml-4 duration-300" src="~/assets/icons/right-arrow.svg" height="20" width="20"/>
            </a>
        </div>
    </div>
</template>

<script setup lang="ts">
import { Props } from 'nuxt/dist/head/runtime/types';

const props: Props = defineProps({
    platforms: Array<any>
});

const articles: Array<any> = await useFetch('/api/medium')
    .then(response => response.data.value.items as Array<any>);

function formatPublishDate(date: string): string {
    return date.split(" ")[0].split("-").reverse().join("-");
}
</script>