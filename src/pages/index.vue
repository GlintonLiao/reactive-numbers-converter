<script setup lang="ts">
import type { IResult } from '~/types'

const { t } = useI18n()
const decimalValue = ref(0)
const isShow = ref(false)
const signed = ref(true)

const hexadecimalVal = computed(() => {
  return decimalValue.value.toString(16)
})

// self-defined base
const base = ref(8)
const extraVal = computed(() => {
  return decimalValue.value.toString(base.value)
})

const updateBase = (e: any) => {
  base.value = parseInt(e.target.value)
}

const updateDecimal = (e: any, base: number) => {
  const res = e.target.value
  if (res.length === 0) return
  const num = parseInt(res, base)
  if (isNaN(num) || (!signed.value && num < 0)) return
  decimalValue.value = num
}

const toFixed = (d = '', length = 8, symbol = 0) => {
  if (d.length < length)
    return symbol + '0'.repeat(length - 1 - d.length) + d
  return d
}

const binaryVal = computed(() => {
  const two = Math.abs(decimalValue.value).toString(2)
  if (decimalValue.value >= 0)
    return toFixed(two, 8, 0)
  else
    return toFixed(two, 8, 1)
})

const onesComplement = computed(() => {
  if (decimalValue.value >= 0) {
    return binaryVal.value
  } else {
    if (signed) {
      let data = ''
      for (let index = 0; index < 8; index++) {
        const item = binaryVal.value[index]
        if (index === 0) data += item
        else data += Math.abs(+item - 1)
      }
      return data
    } else {
      const num = ~decimalValue.value
      return num.toString(2)
    }
  }
})

const twosComplement = computed(() => {
  if (decimalValue.value >= 0)
    return binaryVal.value
  const valid = onesComplement.value.slice(1)
  const validTenComplete = parseInt(valid, 2) + 1
  const validTwoComplete = toFixed(
    validTenComplete.toString(2),
    8,
    1,
  )
  return validTwoComplete
})

const onesToDecimal = (e: any) => {
  const res = parseInt(e.target.value)
  const origin = ~res
  if (!isNaN(res))
    decimalValue.value = origin
}

const twosToDecimal = (e: any) => {
  const res = parseInt(e.target.value)
  const origin = ~res + 1
  if (!isNaN(res))
    decimalValue.value = origin
}

const results = reactive<IResult[]>([])

const save = () => {
  results.push({
    decimalValue: decimalValue.value,
    hexadecimalVal: hexadecimalVal.value,
    extraVal: extraVal.value,
    binaryVal: binaryVal.value,
    onesComplement: onesComplement.value,
    twosComplement: twosComplement.value,
    base: base.value,
  })
}

const clear = () => {
  decimalValue.value = 0
}

const handleDrawer = () => {
  isShow.value = !isShow.value
}
</script>

<template>
  <Transition name="fade">
    <Drawer v-show="isShow" :close-drawer="handleDrawer" :results="results" />
  </Transition>
  <div>
    <div i-carbon-data-reference text-4xl inline-block />
    <p>
      <a rel="noreferrer" href="https://github.com/antfu/vitesse-lite" target="_blank">
        {{ t('intro.desc') }}
      </a>
    </p>
    <p>
      <em text-sm op75>All in one</em>
    </p>

    <div py-3 />

    <div flex justify-center space-x-15 mb-10>
      <div flex flex-col items-center space-y-3>
        <div text-left>
          <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Decimal</label>
          <input
            id="input"
            :value="decimalValue"
            placeholder="Enter a number"
            type="text"
            autocomplete="false"
            p="x-4 y-2"
            w="300px"
            text="center"
            bg="gray-50 dark:gray-700"
            border="~ rounded gray-200 dark:gray-700"
            outline="none active:none"
            @input="e => updateDecimal(e, 10)"
          >
        </div>
        <div text-left>
          <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Hexadecimal</label>
          <input
            id="input"
            :value="hexadecimalVal"
            placeholder="Enter a number"
            type="text"
            autocomplete="false"
            p="x-4 y-2"
            w="300px"
            text="center"
            bg="gray-50 dark:gray-700"
            border="~ rounded gray-200 dark:gray-700"
            outline="none active:none"
            @input="e => updateDecimal(e, 16)"
          >
        </div>
        <div text-left>
          <div flex justify-between items-start>
            <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Self-defined</label>
            <select
              id="base" class="bg-gray-50 text-gray-900 text-xs rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-15 h-5 px-2 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
              @change="updateBase"
            >
              <option value="8" selected>
                8
              </option>
              <option v-for="i in 29" :key="i" :value="i + 1">
                {{ i + 1 }}
              </option>
            </select>
          </div>
          <input
            id="input"
            :value="extraVal"
            type="text"
            autocomplete="false"
            p="x-4 y-2"
            w="300px"
            text="center"
            bg="gray-50 dark:gray-700"
            border="~ rounded gray-200 dark:gray-700"
            outline="none active:none"
            @input="e => updateDecimal(e, base)"
          >
        </div>
      </div>
      <div flex flex-col items-center space-y-3>
        <div text-left>
          <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Binary</label>
          <input
            id="input"
            :value="binaryVal"
            type="text"
            autocomplete="false"
            p="x-4 y-2"
            w="300px"
            text="center"
            bg="gray-50 dark:gray-700"
            border="~ rounded gray-200 dark:gray-700"
            outline="none active:none"
            @input="e => updateDecimal(e, 2)"
          >
        </div>
        <div text-left>
          <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">One's complement</label>
          <input
            id="input"
            :value="onesComplement"
            placeholder="Enter a number"
            type="text"
            autocomplete="false"
            p="x-4 y-2"
            w="300px"
            text="center"
            bg="gray-50 dark:gray-700"
            border="~ rounded gray-200 dark:gray-700"
            outline="none active:none"
            @input="onesToDecimal"
          >
        </div>
        <div text-left>
          <div flex justify-between>
            <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white flex-grow-2">Two's complement</label>
            <label for="AcceptConditions" class="relative h-5 w-10 cursor-pointer">
              <input id="AcceptConditions" v-model="signed" type="checkbox" class="peer sr-only">
              <span class="absolute inset-0 rounded-full bg-gray-300 transition peer-checked:bg-blue-500" />
              <span class="absolute inset-0 m-1 h-3 w-3 rounded-full bg-white transition peer-checked:translate-x-5" />
            </label>
            <span text-sm ml-2>signed</span>
          </div>
          <input
            id="input"
            :value="twosComplement"
            type="text"
            autocomplete="false"
            p="x-4 y-2"
            w="300px"
            text="center"
            bg="gray-50 dark:gray-700"
            border="~ rounded gray-200 dark:gray-700"
            outline="none active:none"
            @input="twosToDecimal"
          >
        </div>
      </div>
    </div>

    <div>
      <button
        class="m-3 text-sm btn"
        @click="save"
      >
        Save
      </button>
      <button
        class="m-3 text-sm btn bg-teal7 hover:bg-teal8"
        @click="handleDrawer"
      >
        View History
      </button>
      <button
        class="m-3 text-sm btn bg-orange7 hover:bg-orange8"
        @click="clear"
      >
        Clear
      </button>
    </div>
  </div>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s ease;
}
.fade-enter-from,
.fade-leave-to {
  left: -40%;
}
</style>
