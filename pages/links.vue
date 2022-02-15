<template>
  <div>
    <table>
      <tbody>
        <tr v-for="link in links" :key="link.title">
          <td class="pl-3"><span>●</span></td>
          <td>
            <p class="ma-0 py-3">
              <a :href="link.to" target="_brank">{{ link.title }}</a>
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
  links: { title: string; to: string }[]
}
export default Vue.extend({
  components: {},
  data(): DataType {
    return {
      links: [
        { title: '東京農工大学', to: 'https://www.tuat.ac.jp/' },
        {
          title: '阿部寛のホームページ',
          to: 'http://abehiroshi.la.coocan.jp/',
        },
      ],
    }
  },
})
</script>

<style lang="scss" scoped></style>
