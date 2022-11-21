<script setup lang="ts">
const name = $ref('')

const router = useRouter()
const go = () => {
  if (name)
    router.push(`/hi/${encodeURIComponent(name)}`)
}
const { t } = useI18n()
</script>

<template>
  <div>
    <div i-carbon-campsite text-4xl inline-block />
    <p>
      <a rel="noreferrer" href="https://github.com/antfu/vitesse-lite" target="_blank">
        {{ t('intro.desc') }}
      </a>
    </p>
    <p>
      <em text-sm op75>All in one without clicking</em>
    </p>

    <div py-3 />

    <div flex justify-center space-x-15 mb-10>
      <div flex flex-col items-center space-y-3>
        <div text-left>
          <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Decimal</label>
          <input
            id="input"
            v-model="name"
            placeholder="Enter a number"
            type="text"
            autocomplete="false"
            p="x-4 y-2"
            w="300px"
            text="center"
            bg="gray-50 dark:gray-700"
            border="~ rounded gray-200 dark:gray-700"
            outline="none active:none"
          >
        </div>
        <div text-left>
          <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Octal</label>
          <input
            id="input"
            v-model="name"
            placeholder="Enter a number"
            type="text"
            autocomplete="false"
            p="x-4 y-2"
            w="300px"
            text="center"
            bg="gray-50 dark:gray-700"
            border="~ rounded gray-200 dark:gray-700"
            outline="none active:none"
          >
        </div>
        <div text-left>
          <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Hexadecimal</label>
          <input
            id="input"
            v-model="name"
            placeholder="Enter a number"
            type="text"
            autocomplete="false"
            p="x-4 y-2"
            w="300px"
            text="center"
            bg="gray-50 dark:gray-700"
            border="~ rounded gray-200 dark:gray-700"
            outline="none active:none"
          >
        </div>
      </div>
      <div flex flex-col items-center space-y-3>
        <div text-left>
          <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Binary</label>
          <input
            id="input"
            v-model="name"
            placeholder="Enter a number"
            type="text"
            autocomplete="false"
            p="x-4 y-2"
            w="300px"
            text="center"
            bg="gray-50 dark:gray-700"
            border="~ rounded gray-200 dark:gray-700"
            outline="none active:none"
          >
        </div>
        <div text-left>
          <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">One's complement</label>
          <input
            id="input"
            v-model="name"
            placeholder="Enter a number"
            type="text"
            autocomplete="false"
            p="x-4 y-2"
            w="300px"
            text="center"
            bg="gray-50 dark:gray-700"
            border="~ rounded gray-200 dark:gray-700"
            outline="none active:none"
          >
        </div>
        <div text-left>
          <div flex justify-between>
            <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white flex-grow-2">Two's complement</label>
            <label for="AcceptConditions" class="relative h-5 w-10 cursor-pointer">
              <input id="AcceptConditions" type="checkbox" class="peer sr-only">
              <span class="absolute inset-0 rounded-full bg-gray-300 transition peer-checked:bg-blue-500" />
              <span class="absolute inset-0 m-1 h-3 w-3 rounded-full bg-white transition peer-checked:translate-x-5" />
            </label>
            <span text-sm ml-2>signed</span>
          </div>
          <input
            id="input"
            v-model="name"
            placeholder="Enter a number"
            type="text"
            autocomplete="false"
            p="x-4 y-2"
            w="300px"
            text="center"
            bg="gray-50 dark:gray-700"
            border="~ rounded gray-200 dark:gray-700"
            outline="none active:none"
          >
        </div>
      </div>
    </div>

    <div>
      <button
        class="m-3 text-sm btn"
        :disabled="!name"
        @click="go"
      >
        Save
      </button>
    </div>
  </div>
</template>
