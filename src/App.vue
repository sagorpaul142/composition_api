<script setup>
import {ref, computed} from 'vue'

const header = ref("Shopping List App")
const characterCount = computed(() => {
  return newItem.value.length
})
const reverseItems = computed(() => [...items.value].reverse())
const items = ref([
  {
    id: 1,
    label: "10 party hats",
    purchased: true,
    highPriority: false
  },
  {
    id: 2,
    label: "2 board games",
    purchased: true,
    highPriority: false
  },
  {
    id: 3,
    label: "20 cups",
    purchased: false,
    highPriority: true
  }
])
const newItem = ref("")
const priority = ref(false)
const editing = ref(false)

const iceCreamFlavors = ref([])

const saveItem = () => {
  items.value.push({
    id: items.value.length + 1,
    label: newItem.value,
    highPriority: priority.value
  })
  newItem.value = ""
  priority.value = false
}

const doEdit = (e) => {
  editing.value = e
  newItem.value = ""
  priority.value = false
}

const togglePurchased = (item) => {
  item.purchased = !item.purchased
}

</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button
        class="btn btn-cancel"
        v-if="editing"
        @click="doEdit(false)"
    >
      Cancel
    </button>

    <button
        class="btn btn-primary"
        @click="doEdit(true)"
        v-else
    >
      Add Item
    </button>
  </div>
  <!--  <a v-bind:href="newItem" target="_blank">Dynamic Link</a>-->
  <form
      class="add-item-form"
      v-if="editing"
      @submit.prevent="saveItem"
  >
    <input
        placeholder="Add an item"
        type="text"
        v-model.trim="newItem"
    >
    <label>
      <input v-model="priority" type="checkbox"> High Priority
    </label>
    <button
        class="btn btn-primary"
        :disabled="newItem.length < 5"
    >
      Save Item
    </button>
  </form>

  <p v-if="editing">
    {{ characterCount }} / 200
  </p>

  <!--  Priority:-->
  <!--  <label>-->
  <!--    <input type="radio" value="low" v-model="priority">-->
  <!--    Low-->
  <!--  </label>-->

  <!--  <label>-->
  <!--    <input type="radio" value="high" v-model="priority">-->
  <!--    High-->
  <!--  </label>-->

  <!--  <select v-model="priority">-->
  <!--    <option value="low">Low</option>-->
  <!--    <option value="high">High</option>-->
  <!--  </select>-->


  <br/>

  <!--  <label>-->
  <!--    <input type="checkbox" v-model="iceCreamFlavors" value="vanilla">-->
  <!--    Vanilla-->
  <!--  </label>-->
  <!--  <label>-->
  <!--    <input type="checkbox" v-model="iceCreamFlavors" value="chocolate">-->
  <!--    Chocolate-->
  <!--  </label>-->
  <!--  <label>-->
  <!--    <input type="checkbox" v-model="iceCreamFlavors" value="strawberry">-->
  <!--    Strawberry-->
  <!--  </label>-->
  <!--  {{ iceCreamFlavors }}-->

  <ul>
    <li
        v-for="(item, index) in reverseItems"
        :key="item.id"
        class="static-class"
        @click="togglePurchased(item)"
        :class="{
          strikeout: item.purchased,
          priority: item.highPriority
        }"
    >
      {{ item.label }}
    </li>

    <!--    <li-->
    <!--        v-for="item in items"-->
    <!--        :key="item.id"-->
    <!--        :class='[-->
    <!--            {strikeout:item.purchased},-->
    <!--            {priority:item.highPriority}-->
    <!--           ]'-->
    <!--    >-->
    <!--      {{ item.label }}-->
    <!--    </li>-->
  </ul>

  <div class="" v-if="!items.length">
    Nothing to see here
  </div>
</template>

<style scoped>
</style>
