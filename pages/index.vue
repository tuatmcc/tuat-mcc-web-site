<template>
  <div>
    <div>
      <h1 class="title-mcc">農工大MCCのホームページ</h1>
    </div>
    <div class="grid">
      <div>
        <v-img
          src="http://abehiroshi.la.coocan.jp/abe-top-20190328-2.jpg"
        ></v-img>
      </div>
      <div>
        <News />
      </div>
    </div>
  </div>
</template>
<script>
import Vue from 'vue'
import News from '~/components/top/news.vue'

export default Vue.extend({
  components: { News },
})
</script>

<style lang="scss" scoped>
.title-mcc {
  font-size: 32px;
  text-align: center;
}
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
</style>
