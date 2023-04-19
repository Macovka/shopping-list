<script setup>
  import { ref, computed } from 'vue'

  const header = ref('Shopping List App')
  const editing = ref(false)
  const items = ref([
    {
      id: 1, 
      label: '10 party hats', 
      purchased: true,
      highPriority: false
    }, 
    {
      id: 2, 
      label: '2 board games', 
      purchased: true,
      highPriority: false
    }, 
    {
      id: 3, 
      label: '20 cups', 
      purchased: false,
      highPriority: true
    }
  ])
  const newItem = ref("")
  const newItemHighPriority = ref(false)

  const saveItem = () => {
    items.value.push(
      {
        id: items.value.length + 1, 
        label: newItem.value,
        highPriority: newItemHighPriority.value
      })
    newItem.value = ""
    newItemHighPriority.value = false
  }
  const doEdit = (e) => {
    editing.value = e
    newItem.value = ""
  }
  const togglePurchesed = (item) => {
    item.purchased = !item.purchased
  }

  const reversedItems = computed(() => [...items.value].reverse())
</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button 
      v-if="editing" 
      class="btn"
      @click="doEdit(false)"
    >
      Cancel
    </button>
    <button 
      v-else 
      class="btn btn-primary"
      @click="doEdit(true)"
    >
      Add Item
    </button>
  </div>
  <form 
    v-if="editing"
    class="add-item-form"
    @submit.prevent="saveItem"
  >
    <input 
      type="text" 
      placeholder="Add an item" 
      v-model.trim="newItem"
    >
    <label>
      <input 
        type="checkbox" 
        v-model="newItemHighPriority"
      >
      High Priority
    </label>
    <button 
      class="btn btn-primary"
      :disabled="newItem.length < 5"
    >
      Save Item
    </button>
  </form>
  
  <ul>
    <li 
      v-for="(item, index) in reversedItems" 
      :key="item.id"
      :class="{
        strikeout: item.purchased, 
        priority: item.highPriority}"
      @click="togglePurchesed(item)"
    >
      {{ item.label }}
    </li>
  </ul>
  <p v-if="!items.length">
    Shopping List is empty
  </p>
</template>


