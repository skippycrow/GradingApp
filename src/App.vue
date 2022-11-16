<template>
  <div id="app" class="bg-gray-200">
    <h1>Grading App</h1>
    Total Score
    <input class="form-control w-30" v-model="totalVal" />
    <hr />
    <template v-for="item in lineItems" :key="item.id">
      <div class="row">
        <div class="form-check col-1 mt-3">
          <input
            class="form-check-input"
            type="checkbox"
            value=""
            v-model="item.isShown"
          />
        </div>
        <div class="col-2">
          <input
            class="form-control m-2"
            :value="prefixNumber(item.amount)"
            @change="(event) => (item.amount = Number(event.target.value))"
            type="text"
            placeholder="Points Deducted"
          />
        </div>

        <input
          class="form-control col m-2"
          v-model.trim="item.issue"
          type="text"
          placeholder="Problem Here"
        />
        <button class="btn btn-danger col-1 m-2" @click="removeItem(item.id)">
          x
        </button>
      </div>
    </template>
    <br />
    <button class="btn btn-primary me-2" @click="addItem()">Add Line</button>
    <button class="btn btn-warning" @click="clearChecked()">
      Clear Checked
    </button>
    <br />
    <br />
    <textarea rows="12" cols="60" disabled>{{ prettyPrint }}</textarea>
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
      { isShown: true, issue: 'for ) is for dweebs', amount: -5, id: '1' },
      { isShown: true, issue: 'Double quotes', amount: -15, id: '2' },
      { isShown: true, issue: 'Super Early Bird', amount: 5, id: '3' },
      { isShown: false, issue: '', amount: 0, id: '4' },
    ])
    let msg = 'hello world'

    const totalVal = ref(0)

    const addItem = () => {
      const ids = lineItems.value.map((object) => {
        return object.id
      })

      const max = Math.max(...ids)

      lineItems.value.push({ isShown: false, issue: '', amount: 0, id: max })
    }

    const clearChecked = () => {
      lineItems.value.map((item) => (item.isShown = false))
    }

    const removeItem = (id) => {
      let index = lineItems.value.findIndex((item) => item.id == id)
      lineItems.value.splice(index, 1)
    }

    const prefixNumber = (num) => {
      if (num < 0) return num

      if (num == 0) return '-0'

      return `+${num}`
    }

    const prettyPrint = computed(() => {
      let str = ''
      let sum = 0
      for (let item of lineItems.value) {
        if (item.isShown) {
          str += `${prefixNumber(item.amount)} ${item.issue} \n`
          sum += Number(item.amount)
        }
      }
      str += `\n===\nTotal: ${Number(totalVal.value) + Number(sum)}`
      return str
    })

    return {
      msg,
      lineItems,
      addItem,
      removeItem,
      prettyPrint,
      totalVal,
      clearChecked,
      prefixNumber,
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
