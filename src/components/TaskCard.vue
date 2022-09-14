<template>
  <div @click="changeShow" class="task-card my-style">
    <div>
      <h4>{{model.title}}</h4>
      <div v-show="this.isShow" class="description">
        <li v-for="(value, index) in model.description" :key="index">
          {{value}}
          <button class="delete-item">Удалить</button>
        </li>
      </div>
      <input v-on:keyup.enter="onAddDescription" v-model="description1" placeholder="Description" type="text">
    </div>
    <div>
      <p>{{ this.date }}</p>
      <span v-if="model.importance">{{ 'Срочная' }}</span>
      <span v-else>{{ 'Обычная' }}</span>
      <button @click="emitOnDone" v-if="!model.status">В работе</button>
      <button @click="emitOnRemove" v-else>Выполнена</button>
    </div>
  </div>
</template>

<script>
import {ref} from "vue";
import moment from "moment";

export default {
  name: "TaskCard",
  data() {
    return {
      time: 0,
      isShow: false,
      date: moment().format('DD.MM HH:mm')
    }
  },
  props: {
    model: {
      required: true,
      default: {
        id: 0,
        title: 'Create Video',
        description: 'And upload on YouTUbe',
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
    changeShow() {
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
  margin: 20px;
}
.task-card {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.description {
  margin-bottom: 15px;
}
.delete-item{
  margin-left: 30px;
}
</style>