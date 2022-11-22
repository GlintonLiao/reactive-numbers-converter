<script setup lang="ts">
interface Props {
  closeDrawer: () => any
}
const props = defineProps<Props>()

const closeDrawer = () => {
  props.closeDrawer()
}
</script>

<template>
  <div fixed z-50 top-0 left-0 transition overflow-hidden>
    <div fixed w-100vw h-100vh bg-transparent z-1 op-50 @click="closeDrawer" />
    <div id="abc" class="fixed z-40 h-screen p-4 overflow-y-auto bg-white w-90 dark:bg-gray-800 transition" tabindex="-1">
      <ul role="list" class="divide-y divide-gray-200 dark:divide-gray-700">
        <li class="px-5 py-3 sm:py-4 shadow-lg bg-gray9 rounded-lg">
          <Result />
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
</style>
