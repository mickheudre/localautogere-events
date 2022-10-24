<template>
    <div >
        <div class="mb-6" v-for="block in events.results" :key="block.id">
            <h3 class="capitalize font-semibold"> {{ dateToString(block.properties.Date.date.start) }}</h3>
            <h2 class="">
                <span  v-for="title in block.properties.Name.title" :key="title.id">
                    {{ title.plain_text }}
                </span>
            </h2>
            <p class="italic">
                <span  v-for="title in block.properties.Description.rich_text" :key="title.id">
                    {{ title.plain_text }}
                </span>
            </p>
            
            <!-- <p v-for=" text in block.paragraph.text" :key="text.id">
                <span v-if="text.text.link === null"> {{ text.text.content }}</span>
                <span v-else> <a target="_parent" :href="text.text.link.url">{{ text.text.content }}</a></span>
            </p> -->
        </div>
    </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
    name: 'IndexPage',
    async asyncData({ $axios }) {
        
        const data = {
            "sorts": [
            {
                "property": "Date",
                "direction": "ascending"
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