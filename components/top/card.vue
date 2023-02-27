<template>
  <nuxt-link :to="path" class="link"
    ><p>{{ title }}</p></nuxt-link
  >
</template>
<script>
import Vue from 'vue'
export default Vue.extend({
  props: {
    path: {
      type: String,
      required: true,
    },
    title: {
      type: String,
      required: true,
    },
  },
})
</script>

<style lang="scss" scoped>
.link {
  text-decoration-line: none;
  color: black;
  p {
    margin: 5px;
  }
}
</style>
