<template>
  <div>
    <div v-for="block in data.results" :key="block.id">
      <div v-if="block.type === 'paragraph'">
        <p v-for=" text in block.paragraph.text" :key="text.id">
          <span v-if="text.text.link === null"> {{ text.text.content }}</span>
          <span v-else> <a target="_parent" :href="text.text.link.url">{{ text.text.content }}</a></span>
        </p>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'IndexPage',
  async asyncData({ $axios }) {
    const data = await $axios.$get("https://api.notion.com/v1/blocks/1c4a2d6206d84417a3fd020b7a988f01/children", {});
    return { data };
  },
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