<template>
  <nuxt-link :to="path" class="link">
    <v-card width="100%" height="100%" class="v-card">
      <v-card-title>{{ title }}</v-card-title>
      <v-card-subtitle
        ><time>{{ date }}</time></v-card-subtitle
      >

      <v-card-text>
        <p class="description ma-0">
          {{ description }}
        </p>
      </v-card-text>
      <div class="tags px-3 pb-1">
        <v-chip-group column>
          <v-chip> {{ category }}</v-chip>
        </v-chip-group>
      </div>
    </v-card>
  </nuxt-link>
</template>

<script lang="ts">
import Vue from 'vue'
// import Tag from '~/components/Tag'
export default Vue.extend({
  props: {
    title: {
      type: String,
      required: true,
    },
    date: {
      type: String,
      required: true,
    },
    category: {
      type: String,
      required: true,
    },
    path: {
      type: String,
      required: true,
    },
    description: {
      type: String,
      required: false,
      default: '',
    },
  },
})
</script>

<style lang="scss" scoped>
.v-card {
  display: flex;
  width: 100%;
  flex-direction: column;
}
.link {
  text-decoration-line: none;
}
.tags {
  margin-top: auto;
}
.description {
  overflow: hidden;
  display: -webkit-box; // webkitの機能を使うようにする
  -webkit-box-orient: vertical; // 縦に折り返す
  -webkit-line-clamp: 2; // 3行まで表示する
}
</style>
