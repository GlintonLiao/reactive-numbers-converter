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

    <div py-3 />

    <div flex justify-center space-x-15>
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
            w="250px"
            text="center"
            bg="transparent"
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
            w="250px"
            text="center"
            bg="transparent"
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
            w="250px"
            text="center"
            bg="transparent"
            border="~ rounded gray-200 dark:gray-700"
            outline="none active:none"
          >
        </div>
        <div text-left>
          <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Decimal</label>
          <input
            id="input"
            v-model="name"
            placeholder="Enter a number"
            type="text"
            autocomplete="false"
            p="x-4 y-2"
            w="250px"
            text="center"
            bg="transparent"
            border="~ rounded gray-200 dark:gray-700"
            outline="none active:none"
          >
        </div>
      </div>
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
            w="250px"
            text="center"
            bg="transparent"
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
            w="250px"
            text="center"
            bg="transparent"
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
            w="250px"
            text="center"
            bg="transparent"
            border="~ rounded gray-200 dark:gray-700"
            outline="none active:none"
          >
        </div>
        <div text-left>
          <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Decimal</label>
          <input
            id="input"
            v-model="name"
            placeholder="Enter a number"
            type="text"
            autocomplete="false"
            p="x-4 y-2"
            w="250px"
            text="center"
            bg="transparent"
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
        Go
      </button>
    </div>
  </div>
</template>
