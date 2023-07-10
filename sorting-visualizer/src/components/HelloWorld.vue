<script setup>
import { ref } from 'vue' // import ref from vue (ref is a function that returns a reactive object)

// define props defines the props that are passed to the component
defineProps({ 
  msg: String, // for example, this component takes in a prop called msg of type string
})

const count = ref(0) // count is a reactive object that can be used in the template (a reactive object is an object that can be used in the template and when it changes, the template is updated)
</script>

<!-- the template is the html that is rendered when this component is used -->
<template> 
  <!-- the msg prop is used here in the template -->
  <h1>{{ msg }}</h1>

  <!-- This is a card div which holds a button with the count and a string of text -->
  <div class="card">
    <!-- the count reactive object is used here in the template -->
    <button type="button" @click="count++">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR 
      <!--  HMR stands for hot module replacement. It is a feature of webpack that allows all kinds of modules to be updated at runtime without the need for a full refresh. -->
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
    <a href="https://github.com/vuejs/language-tools" target="_blank">Volar</a>
    in your IDE for a better DX
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>

<!-- style scoped means that the styles only apply to this component -->
<style scoped>
.read-the-docs {
  color: #888;
}
</style>
