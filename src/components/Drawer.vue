<script setup lang="ts">
interface IResult {
  decimalValue: number
  hexadecimalVal: string
  extraVal: string
  binaryVal: string
  onesComplement: string
  twosComplement: string
  base: number
}
interface Props {
  closeDrawer: () => any
  results: IResult[]
}
const props = defineProps<Props>()
const items = props.results

const closeDrawer = () => {
  props.closeDrawer()
}
</script>

<template>
  <div fixed z-50 top-0 left-0 transition overflow-hidden>
    <div fixed w-100vw h-100vh bg-transparent z-1 op-50 @click="closeDrawer" />
    <div id="abc" class="fixed z-40 h-screen px-6 overflow-y-auto bg-gray1 w-150 dark:bg-gray-800 transition" tabindex="-1">
      <ul role="list" class="divide-y divide-gray-200 dark:divide-gray-700">
        <li v-for="(item, idx) in items" :key="idx" class="px-5 py-3 my-6 sm:py-4 shadow-lg bg-white dark:bg-gray9 rounded-lg">
          <Result :item="item" />
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
</style>
