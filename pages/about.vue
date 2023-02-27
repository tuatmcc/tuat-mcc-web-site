<template>
  <article>
    <div>
      <nuxt-content class="markdown-body" :document="article" />
    </div>
  </article>
</template>

<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  async asyncData({ $content }) {
    const query = $content('about')
    const article = await query.fetch()
    return { article }
  },
})
</script>
