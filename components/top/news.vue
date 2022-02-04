<template>
  <div>
    <div>
      <h3 class="text-center">★★★ 最新情報 ★★★</h3>
    </div>
    <v-divider />
    <div>
      <h4>・ドラマ</h4>
      <p>2023年 大河ドラマ(第62作)New!</p>
      <p>日曜劇場「DCU」New! 2022年1月スタート</p>
    </div>
    <v-divider />
    <div>
      <h4>・映画</h4>
      <p>2023年 大河ドラマ(第62作)New!</p>
      <p>日曜劇場「DCU」New! 2022年1月スタート</p>
      <news-card />
    </div>
  </div>
</template>
<script>
import Vue from 'vue'
import newsCard from '~/components/top/newsCard.vue'

export default Vue.extend({
  components: { newsCard },
})
</script>

<style lang="scss" scoped></style>
