<template>
  <main>
    <ul class="task-list my-list">
      <TaskInput @onAddTask="addTask" @onSort="sort" @onInput="onInput"></TaskInput>
      <li v-for="item in search" :key="item.id">
        <TaskCard @onRemove="removeTask(item.id)" @onDone="setDoneTask(item.id)" @onAddDescription="val => addDescription(val.description, item.id)" :model="item"></TaskCard>
      </li>
    </ul>
  </main>
</template>

<script>
import TaskCard from "@/components/TaskCard";
import TaskInput from "@/components/TaskInput";
import { ref } from 'vue';

export default {
  name: 'App',
  components: {
    TaskInput,
    TaskCard
  },
  data() {
    return {
      inputSearch: ''
    }
  },
  setup() {
    const taskList = ref([{id:0, title: 'Create Video', description: ['And upload on YT'], importance: 'Обычная', status: false}]);
    let sorting = true;

    const addTask = ({title, checked}) => {
      if(checked === true) {
        taskList.value.unshift({id: taskList.value.length + 1, title, description: [], importance: 'Срочная', checked, status: false})
      } else {
        taskList.value = [...taskList.value, {id: taskList.value.length + 1, title, description: [], importance: 'Обычная', checked, status: false}]
      }
    }
    const addDescription = (description, id) => {
      taskList.value = taskList.value.map(x => {
        if (x.id === id)
          x.description.push(description)
        return x
      })
    }

    const setDoneTask = (id) => {
      taskList.value = taskList.value.map(x => {
        if (x.id === id)
          x.status= true
        return x
      })
    }

    const removeTask = (id) => {
      taskList.value = taskList.value.filter(x => x.id !== id)
    }
    const sort = () => {
      if (sorting) {
        taskList.value.sort((a,b) => a.title.toLowerCase() > b.title.toLowerCase() ? 1 : -1)
        sorting = false
      }
      else {
        taskList.value.sort((a,b) => a.title.toLowerCase() < b.title.toLowerCase() ? 1 : -1)
        sorting = true
      }
    }

    return {
      taskList,
      addTask,
      addDescription,
      setDoneTask,
      removeTask,
      sort
    }
  },
  computed: {
    search() {
      return this.taskList.filter(list => {
        return list.title.indexOf(this.inputSearch) !== -1
      })
    }
  },
  methods: {
    onInput(val) {
      this.inputSearch = val
    }
  }
}
</script>

<style>
#app {
  font-family: "JetBrains Mono", monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 0;
  padding: 0;
}
main {
  max-width: 450px;
  margin: 0 auto;
  padding: 10px 0;
}
.task-list {
  list-style: none;
}

.my-list {
  display: grid;
  grid-template-rows: auto;
  grid-template-columns: 100%;
  grid-gap: 5px;
}
</style>
