<template>
  <div class="content">
    <br />
    <table border="0">
      <tbody>
        <tr v-for="sbj in subjects" :key="sbj.name">
          <td class="pl-3"><span :style="`color:${sbj.color}`">●</span></td>
          <td>
            <p class="ma-0 py-3">
              <nuxt-link :to="sbj.href">{{ sbj.name }}</nuxt-link>
            </p>
          </td>
        </tr>
        <tr></tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
export type DataType = {
  subjects: { name: string; color: string; href: string }[]
}
export default Vue.extend({
  components: {},
  data(): DataType {
    return {
      subjects: [
        { name: 'トップ', color: '#FFCCCC', href: '/' },
        { name: 'MCCについて', color: '#00FF00', href: '/about' },
        { name: '活動内容', color: '#33FFFF', href: '/works' },
        { name: '活動報告', color: '#0099FF', href: '/report' },
        { name: '新入部員募集', color: ' #0000FF', href: '/recruit' },
        { name: 'お役立ちリンク', color: ' #333399', href: '/links' },
        // #CC00CC
      ],
    }
  },
})

/*
export default Vue.extend({
  components: {},
  layout: 'coefontsLayout',
  middleware: 'authenticated',
  data(): DataType {
    return {}
  },
})
*/
</script>
<style lang="scss" scoped>
.content {
  background-color: #f0f0ff;
  height: 100%;
}
</style>
