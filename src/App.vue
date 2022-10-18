<template>
  <div id="app">
    <template v-for="item in lineItems" :key="item.id">
      <input v-model="item.issue" type="text" placeholder="Problem Here" />
      <input
        v-model="item.amount"
        type="number"
        placeholder="Points Deducted"
      />
      <button @click="removeItem(item.id)">x</button>
      <hr />
    </template>
    <br />
    <button @click="addItem()">Add Line</button>
    <br />
    <textarea disabled>{{prettyPrint}}</textarea
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import { ref, computed } from 'vue'

export default {
  name: 'App',
  components: {
    HelloWorld,
  },
  setup() {
    let lineItems = ref([
      { issue: 'sample', amount: -3, id: '1' },
      { issue: 'This', amount: -3, id: '2' },
      { issue: 'That', amount: -3, id: '3' },
      { issue: '', amount: 0, id: '4' },
    ])
    let msg = 'hello world'

    const addItem = () => {
      const ids = lineItems.value.map((object) => {
        return object.id
      })

      const max = Math.max(...ids)

      lineItems.value.push({ issue: '', amount: 0, id: max })
    }

    const removeItem = (id) => {
      let index = lineItems.value.findIndex((item) => item.id == id)
      lineItems.value.splice(index, 1)
    }

    const prettyPrint = computed(() => {
      let str = ''
      for (let item of lineItems.value) {
        str += `${item.issue}: ${item.amount} \n`
      }
      console.log(str)
      return str
    })

    return {
      msg,
      lineItems,
      addItem,
      removeItem,
      prettyPrint,
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
