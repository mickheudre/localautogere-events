<template>
  <div>
    <div v-for="block in actu.results" :key="block.id">
      <div v-if="block.type === 'paragraph'">
        <p v-for=" text in block.paragraph.rich_text" :key="text.id">
          <span v-if="text.text.link === null"> {{ text.text.content }}</span>
          <span v-else> <a target="_parent" :href="text.text.link.url">{{ text.text.content }}</a></span>
        </p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const {data : actu} = await useFetch(
'https://api.notion.com/v1/blocks/1c4a2d6206d84417a3fd020b7a988f01/children', {
headers : {
  'Content-Type': 'application/json',
  'Authorization': `Bearer ${process.env.NOTION_TOKEN}`,
  'Notion-Version': '2022-06-28',
}
})

console.log(actu)
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