---
title: About
---

<div class="text-center">
  <div i-carbon-dicom-overlay class="text-4xl -mb-6 m-auto" />
  <h3>About this Project</h3>
</div>

[Reactive Numbers Convertor](https://github.com/glintonliao/reactive-numbers-converter) is an online tool for numbers conversion. It allows users to enter in any input box and synchronizes conversion results without clicking any button.

## Problem Space

There are many online converters already, but none of them supports reactive conversion. Users can either enter in only one input box, or can enter at many input boxes, but need to click a "Confirm" or "Convert" button to get the result. Also, most of them support only one single feature, such as binary conversion, or binary to hexadecimal conversion, or only signed two's complements.

<h4 text-center>
How might we make a tool to convert everything all in once?
</h4>

## Introducing the Reactive Numbers Converter

In order to realize the real time conversion, we need to form a data model.

### Computed Value

In Vue.js, there is a reactive callback function called **Computed()**. It can compute value by passing in some reactive dependencies values.

```ts
const a = ref(1)
const b = computed(() => {
  return a.value * 2
})

console.log(b.value) // 2
a.value = 2
console.log(b.value) // 4
```

When the dependencies value changed, the computed value will be automatically updated.

It is worth noting that the function will use cached value first when the value didn't change, so it's beneficial for the performance when updating pages.

### Publish-Subscribe Model

After having some way to sync different values, we can form a "Publish-Subscribe" data model.

The key of the model is a "base" value. every other values will depend on this value, which is "subscribe" process. When the base value changes(publish), those value will be noticed to update.

This image shows how data flows:

![data model](/data%20model.jpg)

Therefore, we can convert every values in real time.

### Reactivity in Depth

Before Vue 3, the reactivity was achieved by **Object.defineProperty()**. Vue 3 is using a new, more effected way, which is the **Proxy** object.

```ts
function reactive(obj) {
  return new Proxy(obj, {
    get(target, key) {
      track(target, key)
      return target[key]
    },
    set(target, key, value) {
      target[key] = value
      trigger(target, key)
    }
  })
}
```

Instead of getting or setting the data directly, we use the "getter()" and "setter()". Therefore, when getting access to some data, we can track it and make some additional operations, such as triggering layout update and data synchronization.

## Layout and Input

I set the inputs components in two columns using flex-box layout, one for base conversion, and the other for complements conversion.

In terms of the input, Vue has a convenient input mutual binding attribute called **v-model**, but in this project, because we need to sync multiple values, we need to separate the value and on change function. Every input element has a **:value** bind with a ref in script, and an **@input** function to trigger the update process.

Plus, the project supports multiple languages and dark mode. You can click the icon at the footer to toggle.

## Core Logic

### Convert numbers into different radix

We can use one of the built-in function in JavaScript, **toString()**, this function takes a number, and an option parameter for radix.

Each time we modify the value in input bar, we can convert it into decimal, and update the decimal value, then vue will automatically update every other corresponding values according to the computed methods.

```ts
// need to set some error handling logic
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
```

### Convert numbers into ones' complement and two's complement

First, we need to determined the number of bits for the binary value. In this project, I chose 32-bits, because 32 bits is the number of bits used for integer types in some strongly typed languages.

Then we should come up a way to fix the length of the number.

```ts
// if the number is less than 32 bits, we append 0 to make it 32 bits
const toFixed = (d = '', length = 32, symbol = 0) => {
  if (d.length < length)
    return symbol + '0'.repeat(length - 1 - d.length) + d
  return d
}
```

## Final Takeaways

The project is simple to build, but still contains much content and tech stacks in terms of the front-end engineering.

### Project Tech Stacks

+ TypeScript
+ Vue
  + Composition API(script setup)
+ UnoCSS
+ pnpm
+ Netlify
+ Vite
  + i18n
  + Auto-import

Aside from these, I've learned how to build an app with "Publish-Subscribe" model, which is beneficial for the future as a software engineer.

Hope you Enjoy this project!
