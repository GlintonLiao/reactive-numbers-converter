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

This img shows how data flows:

![data model]()

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

Plus, the project supports multiple languages and dark mode. You can click the icon in footer to toggle.

## Core Logic

### Convert numbers into different base

### Convert numbers into ones' complement and two's complement

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
