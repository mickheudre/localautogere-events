<template>
    <div class="max-h-screen overflow-y-auto">
        <div class="mb-6" v-for="block in events.results"  :key="block.id">
            <div v-if="block.properties.Incoming.formula.boolean"> 
                <h3 class="capitalize font-semibold"> {{ dateToString(block.properties.Date.date.start) }}</h3>
                <span class="italic text-sm"> {{ formatTags(block.properties.Tags.multi_select) }} </span>
                <h2 class="">
                    <span  v-for="title in block.properties.Name.title" :key="title.id">
                        {{ title.plain_text }}
                    </span>
                </h2>
                <p class="">
                    <span  v-for="title in block.properties.Description.rich_text" :key="title.id">
                        {{ title.plain_text }}
                    </span>
                </p>
            </div>
        </div>
        <h3 class="text-3xl font-semibold capitalize mt-12">évènements passés</h3>
        <div class="mb-6" v-for="block in events.results"  :key="block.id">
            <div v-if="!block.properties.Incoming.formula.boolean"> 
                <h3 class="capitalize font-semibold"> {{ dateToString(block.properties.Date.date.start) }}</h3>
                <span class="italic text-sm"> {{ formatTags(block.properties.Tags.multi_select) }} </span>
                <h2 class="">
                    <span  v-for="title in block.properties.Name.title" :key="title.id">
                        {{ title.plain_text }}
                    </span>
                </h2>
                <p class="">
                    <span  v-for="title in block.properties.Description.rich_text" :key="title.id">
                        {{ title.plain_text }}
                    </span>
                </p>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { isLet } from '@babel/types';
import { List } from 'postcss/lib/list';
import Vue from 'vue'

export default Vue.extend({
    name: 'IndexPage',
    async asyncData({ $axios }) {
        
        const data = {
            "sorts": [
            {
                "property": "Date",
                "direction": "descending"
            }
            ]
        }
        
        const events = await $axios.$post("https://api.notion.com/v1/databases/2e87451f6d604576ab1e5b37dea65590/query", data );
        return { events };
    },
    methods: {
        dateToString(date : string) {
            const event = new Date(date)
            return event.toLocaleDateString('fr-FR', { weekday: "long", year: "numeric", month: "long", day: "numeric" })
        },
        formatTags(tags : Array<any>) {
            let tagsList = new Array<string>();
            
            tags.forEach(element => {
                tagsList.push(element.name)
            });
            return tagsList.join(', ')
        }
    }
})
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Source+Sans+Pro:wght@400;600&display=swap');

html {
    overflow: hidden;
}

p {
    font-family: 'Source Sans Pro', sans-serif;
}

a {
    font-family: 'Source Sans Pro', sans-serif;
    font-weight: 600;
    color: black;
}
</style> 