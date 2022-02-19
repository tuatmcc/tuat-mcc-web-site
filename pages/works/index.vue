<template>
  <!-- container -->
  <div class="box">
    <Card
      v-for="(article, index) in articles"
      :key="index"
      :title="article.title"
      :category="article.category"
      :path="article.path"
      :created-at="article.createdAt"
    />
  </div>
</template>
<script lang="ts">
import Vue from 'vue'
import Card from '~/components/news/newsCard.vue'

export default Vue.extend({
  components: { Card },
  async asyncData({ $content }) {
    const query = await $content('works').limit(15)
    const articles = await query.fetch()
    return { articles }
  },
})
</script>

<style lang="scss" scoped>
.box {
  display: grid;
  gap: 15px;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
  grid-auto-rows: 200px;
}
</style>
