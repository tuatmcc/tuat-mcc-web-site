<template>
  <div class="content">
    <table border="0">
      <tbody>
        <tr v-for="sbj in subjects" :key="sbj.title">
          <td class="pl-3"><span :style="`color:${sbj.color}`">●</span></td>
          <td>
            <p class="ma-0 py-3">
              <nuxt-link :to="sbj.to">{{ sbj.title }}</nuxt-link>
            </p>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
export type DataType = {
  subjects: { title: string; color: string; to: string }[]
}
export default Vue.extend({
  components: {},
  data(): DataType {
    return {
      subjects: [
        { title: 'トップ', color: '#FFCCCC', to: '/' },
        { title: 'MCCについて', color: '#00FF00', to: '/about' },
        { title: '活動内容', color: '#33FFFF', to: '/works' },
        { title: '活動報告', color: '#0099FF', to: '/news' },
        { title: '新入部員募集', color: ' #0000FF', to: '/recruit' },
        { title: 'お役立ちリンク', color: ' #333399', to: '/links' },
        // #CC00CC
      ],
    }
  },
})
</script>
<style lang="scss" scoped>
.content {
  background-color: #f0f0ff;
  height: 100%;
}
</style>
