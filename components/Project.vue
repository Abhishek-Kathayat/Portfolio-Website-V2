<template>
    <div class="flex flex-col mx-6 sm:mx-10 lg:mx-0">
        <div class="lg:hidden text-xs uppercase font-semibold tracking-[2px] mb-6"> Projects </div>
        <div class="md:grid md:grid-cols-2 lg:w-fit group">
            <div class="h-[320px] sm:h-[300px] md:h-[350px] lg:w-[225px] lg:h-[480px] xl:w-[285px] xl:h-[420px] 2xl:w-[340px] 2xl:h-[340px] 
                          md:odd:mr-3 lg:odd:mr-2 xl:odd:mr-3 2xl:odd:mr-4 mb-3 sm:mb-4 md:mb-3 lg:mb-2 xl:mb-3 2xl:mb-4
                        flex flex-col bg-cardbgclr p-6 sm:p-8 
                        lg:hover:scale-105 lg:duration-300 lg:group-hover:opacity-50 lg:hover:!opacity-100"
                v-for="repository in repositories" :key="repository.id">
                <div class="text-[11px] tracking-[2px] uppercase"> {{ repository.language }} </div>
                <h1 class="mt-2 font-semibold text-2xl leading-tight"> {{ repository.name }} </h1>
                <p class="mt-2 text-sm text-subclr" v-html="repository.description"> </p>
                <ul class="mt-6 flex whitespace-nowrap flex-wrap">
                    <li class="text-[10px] bg-topicsbgclr rounded-full font-semibold text-white px-2 py-1 mr-2 mb-2"
                        v-for="topic in repository.topics">
                        {{ topic }}
                    </li>
                </ul>
                <div class="mt-auto flex items-center"> 
                    <img src="~/assets/icons/github.svg" height="18" width="18" />
                    <a class="flex items-center" :href="repository.html_url" target="_blank" rel="noopener noreferrer">
                        <span class="mx-2 text-sm"> GitHub </span>
                        <img src="~/assets/icons/external.svg" height="12" width="12"/>
                    </a>
                </div>
            </div>
        </div>
        <div class="w-fit mt-2">
            <a class="flex items-center group" :href="props.platforms && props.platforms[1].url" target="_blank" rel="noopener noreferrer">
                <span class="font-semibold tracking-wide text-sm"> View GitHub Profile </span>
                <img class="ml-2 group-hover:ml-4 duration-300" src="~/assets/icons/right-arrow.svg" height="20" width="20"/>
            </a>
        </div>
    </div>
</template>

<script setup lang="ts">
import { Props } from 'nuxt/dist/head/runtime/types';
import { processDescriptionEmojis } from '~/services/github-service';

const props: Props = defineProps({
    platforms: Array<any>
});

const repositories: Array<any> = await useFetch('/api/github')
    .then(response => response.data.value as Array<any>);

for(const repository of repositories) {
    repository.name = repository.name.replace(/-/g, ' ');
    repository.description = await processDescriptionEmojis(repository.description);
}
</script>