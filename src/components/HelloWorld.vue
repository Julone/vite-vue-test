<script setup lang="ts">
import { ref, render, h } from 'vue'


defineProps<{ msg: string }>()

const count = ref(0)
console.log(count)

const vnode = {
  tag: 'div',
  props: {
    onClick(){
      alert(2)
    }
  },
  children: [
    {tag: 'h2',props: {},children:'f÷ssfdsf'},
    {tag: 'h2',props: {},children:'fs'}
  ]
}

const a = h('div', vnode.props, vnode.children.map(item => typeof item == 'string'?item: h(item.tag, item.props, item.children)));
console.log(a)
render(a, document.body)


let activeEffect
const bucket = new Set()
function  effect(fn) {
  activeEffect = fn
  fn()
}
const data = {text: '24'}
const obj = new Proxy(data, {
  get(target, key) {
    if((activeEffect)) {
      bucket.add(activeEffect)
    }
    return target[key]
  },
  set(target, key, value) {
    target[key] = value
    if(bucket.size) {
      bucket.forEach(fn => fn())
    }
    return true
  }
})

effect(() => {
  document.body.innerHTML = obj.text
})
window.obj = obj
  
</script>

<template>
  <h1>{{ msg }}</h1>

  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
      >create-vue</a
    >, the official Vue + Vite starter
  </p>
  <p>
    Install
    <a href="https://github.com/johnsoncodehk/volar" target="_blank">Volar</a>
    in your IDE for a better DX
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
