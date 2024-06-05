<template>
  <div>
    <h1>Task manager</h1>
    <AddTodo />
  </div>
</template>

<script>
import AddTodo from '@/components/AddTodo.vue'

export default {
  name: 'TodosView',
  components: {
    AddTodo
  }
}
</script>

<style lang='scss'></style>
