<template>
  <div>
    <div>
      <h1 class="title-mcc">農工大MCCのホームページ</h1>
    </div>
    <div class="grid">
      <div class="left">
        <v-img
          width="100%"
          src="https://mononichi.com/worksimg/lan.png"
        ></v-img>
      </div>
      <div class="right">
        <News :articles="articles" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import News from '~/components/top/news.vue'
export default Vue.extend({
  components: { News },
  async asyncData({ $content }) {
    // ニュースと作品の二次元配列を作る
    const folder = ['news', 'works']
    const articles = await Promise.all(
      folder.map(async (el) => {
        const query = await $content(el).limit(5)
        const article = await query.fetch()
        return article
      })
    )
    return { articles }
  },
})
</script>
<style lang="scss" scoped>
.title-mcc {
  font-size: 32px;
  text-align: center;
}
.grid {
  .left {
    max-width: 500px;
    margin: auto;
    min-width: 0;
    width: 100%;
  }
  .right {
    width: 100%;
  }
  display: grid;
  gap: 24px;
  grid-template-columns: 1fr minmax(400px, 1fr);
  @media (max-width: 700px) {
    grid-template-columns: 1fr;
  }
}
</style>
