<script setup lang="ts">
import type { IResult } from '~/types'
const props = defineProps<{
  item: IResult
}>()

const {
  decimalValue,
  hexadecimalVal,
  extraVal,
  binaryVal,
  onesComplement,
  twosComplement,
  base,
} = props.item
</script>

<template>
  <div class="flex items-center space-x-4 justify-between mb-5">
    <div class="min-w-0">
      <p class="text-sm font-medium text-gray-900 truncate dark:text-white">
        Decimal
      </p>
    </div>
    <div class="inline-flex items-center text-base font-semibold text-gray-900 dark:text-white">
      {{ decimalValue }}
    </div>
  </div>
  <div class="flex items-center space-x-4 justify-between mb-5">
    <div class="min-w-0">
      <p class="text-sm font-medium text-gray-900 truncate dark:text-white">
        Hexadecimal
      </p>
    </div>
    <div class="inline-flex items-center text-base font-semibold text-gray-900 dark:text-white">
      {{ hexadecimalVal }}
    </div>
  </div>
  <div class="flex items-center space-x-4 justify-between mb-5">
    <div class="min-w-0">
      <p class="text-sm font-medium text-gray-900 truncate dark:text-white">
        Base-{{ base }}
      </p>
    </div>
    <div class="inline-flex items-center text-base font-semibold text-gray-900 dark:text-white">
      {{ extraVal }}
    </div>
  </div>
  <div class="flex items-center space-x-4 justify-between mb-5">
    <div class="min-w-0">
      <p class="text-sm font-medium text-gray-900 truncate dark:text-white">
        Binary
      </p>
    </div>
    <div class="inline-flex items-center text-base font-semibold text-gray-900 dark:text-white">
      {{ binaryVal }}
    </div>
  </div>
  <div class="flex items-center space-x-4 justify-between mb-5">
    <div class="min-w-0">
      <p class="text-sm font-medium text-gray-900 truncate dark:text-white">
        One's complement
      </p>
    </div>
    <div class="inline-flex items-center text-base font-semibold text-gray-900 dark:text-white">
      {{ onesComplement }}
    </div>
  </div>
  <div class="flex items-center space-x-4 justify-between">
    <div class="min-w-0">
      <p class="text-sm font-medium text-gray-900 truncate dark:text-white">
        Two's complement
      </p>
    </div>
    <div class="inline-flex items-center text-base font-semibold text-gray-900 dark:text-white">
      {{ twosComplement }}
    </div>
  </div>
</template>
