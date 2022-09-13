<template>
  <div class="task-input my-list">
    <input v-model="title" placeholder="Title" type="text">
<!--    <input v-model="description" placeholder="Description" type="text">-->
    <div>
      <input type="checkbox" v-model="checked" placeholder="Importance">
      <label for="checkbox">Важность</label>
    </div>
    <input @input="$emit('onInput', search)" v-model="search" placeholder="Search" type="text">
    <button @click="onAddTask">Add task</button>
    <button @click="onSort">Sort</button>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  name: "TaskInput",
  emits: {
    onAddTask({ title}) {
      if(title === '') {
        alert('Fill some info please')
        return false
      }
      return true
    }
  },
  setup(props, { emit }) {
    const title = ref('')
    const description = ref('')
    const importance = ref('')
    const search = ref('')
    const checked = ref(false)

    const onAddTask = () => {
      emit('onAddTask', {title: title.value, description: description.value, importance: importance.value, checked: checked.value});
      title.value = description.value = '';
      checked.value = false;
    }
    const onSort = () => {
      emit('onSort')
    }

    return {
      title,
      description,
      importance,
      search,
      checked,
      onAddTask,
      onSort
    }
  },
  methods: {
    resetInput(title, description, checked){
      title = description = '';
      // eslint-disable-next-line no-unused-vars
      checked = false;
    },
  }
}
</script>

<style scoped>
.my-list {
  display: grid;
  grid-template-rows: auto;
  grid-template-columns: 100%;
  grid-gap: 5px;
}
.task-input {
  margin: 10px 0;
}
</style>