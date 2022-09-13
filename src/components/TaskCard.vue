<template>
  <div @click="changeShow" class="task-card my-style">
    <div>
      <h4>{{model.title}}</h4>
      <div v-show="this.isShow">
        <li v-for="(value, index) in model.description" :key="index">{{value}}</li>
      </div>
      <input v-on:keyup.enter="onAddDescription" v-model="description1" placeholder="Description" type="text">
    </div>
    <div>
      <p>{{ this.time }}</p>
      <span>{{ model.importance }}</span>
      <button @click="emitOnDone" v-if="!model.status">+</button>
      <button @click="emitOnRemove" v-else>-</button>
    </div>
  </div>
</template>

<script>
import {ref} from "vue";

export default {
  name: "TaskCard",
  data() {
    return {
      checked: true,
      time: 0,
      isShow: false
    }
  },
  mounted() {
    this.startTimer()
  },
  props: {
    model: {
      required: true,
      default: {
        id: 0,
        title: 'Create Video',
        description: 'And upload on YouTUbe',
        importance: "Обычная",
        status: false
      }
    },
    timer: {
      type: Boolean,
      default: false
    }
  },
  setup(props, { emit }) {
    const description1 = ref('')
    const emitOnDone = () => {
      emit('onDone')
    }
    const emitOnRemove = () => {
      emit('onRemove')
    }

    const onAddDescription = () => {
      emit('onAddDescription', {description: description1.value});
    }

    return {
      description1,
      emitOnDone,
      emitOnRemove,
      onAddDescription
    }
  },
  methods: {
    startTimer() {
      setInterval(() => {
        this.time++
      }, 1000)
    },
    changeShow() {
      console.log( this.isShow)
      this.isShow = !this.isShow
    }
  }
}
</script>

<style scoped>
button, input, .my-style {
  border-radius: 5px;
  border: solid 1px lightgrey;
  padding: 10px;
}
span {
  padding-right: 30px;
}
.task-card {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>