<template>
  <div>
    <div>
      <h3 class="text-center">★★★ 最新情報 ★★★</h3>
    </div>
    <v-divider />
    <h4>・活動報告</h4>
    <div v-for="(article, index) in articles[0]" :key="index">
      <news-card :path="article.path" :title="article.title" />
    </div>
    <v-divider />
    <div>
      <h4>・活動内容</h4>
      <div v-for="(article, index) in articles[1]" :key="index">
        <news-card :path="article.path" :title="article.title" />
      </div>
    </div>
  </div>
</template>
<script>
import Vue from 'vue'
import newsCard from '~/components/top/card.vue'

export default Vue.extend({
  components: { newsCard },
  props: {
    articles: {
      type: Array,
      required: true,
    },
  },
})
</script>

<style lang="scss" scoped></style>
