<template>
  <article>
    <h1 class="text-center">
      {{ article.title || '' }}
    </h1>
    <div class="">
      <nuxt-content class="markdown-body" :document="article" />
    </div>
  </article>
</template>

<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  async asyncData({ $content, params }) {
    const query = $content('works', params.slug)
    const article = await query.fetch()
    return { article }
  },
})
</script>
