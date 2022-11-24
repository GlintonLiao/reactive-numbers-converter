<script setup lang="ts">
import { tSInferType } from '@babel/types';
import type { IResult } from '~/types'

const { t } = useI18n()
const decimalValue = ref(0)
const isShow = ref(false)
const signed = ref(true)

const toggleError = (e: Event, flag: boolean) => {
  if (flag) (e.target as Element).classList.add('bg-red-50', 'text-red-900', 'placeholder-red-700', 'dark:text-red-400')
  else (e.target as Element).classList.remove('bg-red-50', 'text-red-900', 'placeholder-red-700', 'dark:text-red-400')
}

const hexadecimalVal = computed(() => {
  return decimalValue.value.toString(16)
})

// self-defined base
const base = ref(8)
const extraVal = computed(() => {
  return decimalValue.value.toString(base.value)
})

const updateBase = (e: any) => {
  const prev = extraVal.value
  base.value = parseInt(e.target.value)
  decimalValue.value = parseInt(prev, base.value)
}

const updateDecimal = (e: any, base: number) => {
  const res = e.target.value
  if (res.length === 0) {
    toggleError(e, false)
    return
  }
  const num = parseInt(res, base)
  if (isNaN(num) || (!signed.value && num < 0)) {
    toggleError(e, true)
    return
  } else {
    toggleError(e, false)
  }
  decimalValue.value = num
}

const toFixed = (d = '', length = 32, symbol = 0) => {
  if (d.length < length)
    return symbol + '0'.repeat(length - 1 - d.length) + d
  return d
}

const binaryVal = computed(() => {
  const two = Math.abs(decimalValue.value).toString(2)
  if (decimalValue.value >= 0)
    return toFixed(two, 32, 0)
  else
    return toFixed(two, 32, 1)
})

const onesComplement = computed(() => {
  if (decimalValue.value >= 0 || !signed.value) return binaryVal.value
  let res = ''
  for (const char of binaryVal.value)
    res += char === '0' ? '1' : '0'
  if (signed)
    return binaryVal.value[0] + res.slice(1)
  else return res
})

const twosComplement = computed(() => {
  if (decimalValue.value >= 0 || !signed.value) return binaryVal.value
  const valid = onesComplement.value.slice(1)
  const validTenComplete = parseInt(valid, 2) + 1
  const validTwoComplete = toFixed(
    validTenComplete.toString(2),
    32,
    1,
  )
  return validTwoComplete
})

const binaryToDecimal = (e: any) => {
  const str = e.target.value
  if (str.length === 0) {
    toggleError(e, false)
    return
  }
  let res = 0
  if (signed.value && str[0] === '1')
    res = -parseInt(str.slice(1), 2)
  else
    res = parseInt(str, 2)
  if (isNaN(res)) {
    toggleError(e, true)
    return
  } else { toggleError(e, false) }
  decimalValue.value = res
}

const onesToDecimal = (e: any) => {
  const str = e.target.value
  if (str.length === 0) {
    toggleError(e, false)
    return
  }
  console.log(str);
  let res = 0
  if (signed.value && str[0] === '1') {
    res = -(~parseInt(str.slice(1), 2))

  }
  else
    res = parseInt(str, 2)
  if (isNaN(res)) {
    toggleError(e, true)
    return
  } else { toggleError(e, false) }
  decimalValue.value = res
}

const twosToDecimal = (e: any) => {
  const res = parseInt(e.target.value)
  const origin = ~res + 1
  if (!isNaN(res))
    decimalValue.value = origin
}

const results = reactive<IResult[]>([])
const success = ref(false)

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
  success.value = true
  setTimeout(() => {
    success.value = false
  }, 1200)
}

const clear = () => {
  decimalValue.value = 0
}

const handleDrawer = () => {
  isShow.value = !isShow.value
}
</script>

<template>
  <Transition name="pop">
    <Success v-show="success" class="fixed mx-auto left-0 right-0 w-100 z-10 top-80" />
  </Transition>
  <Transition name="fade">
    <Drawer v-show="isShow" :close-drawer="handleDrawer" :results="results" />
  </Transition>
  <div>
    <div i-carbon-data-reference text-4xl inline-block />
    <p>
      <a rel="noreferrer" href="https://github.com/glintonliao/reactive-numbers-converter" target="_blank">
        {{ t('intro.desc') }}
      </a>
    </p>
    <p>
      <em text-sm op75>All in one, fully reactive</em>
    </p>

    <div py-3 />

    <div flex justify-center space-x-15 mb-10>
      <div flex flex-col items-center space-y-3>
        <div text-left>
          <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Decimal</label>
          <input
            id="inputd"
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
            placeholder="Enter a number"
            autocomplete="false"
            p="x-4 y-2"
            w="400px"
            text="center"
            bg="gray-50 dark:gray-700"
            border="~ rounded gray-200 dark:gray-700"
            outline="none active:none"
            @input="binaryToDecimal($event)"
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
            w="400px"
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
            placeholder="Enter a number"
            type="text"
            autocomplete="false"
            p="x-4 y-2"
            w="400px"
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

.pop-enter-active,
.pop-leave-active {
  opacity: 1;
  transition: all 0.5s ease;
}
.pop-enter-from,
.pop-leave-to {
  opacity: 0;
}
</style>
