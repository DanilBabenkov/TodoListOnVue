<template>
  <div class="task-card my-style">
    <div>
      <h4>{{model.title}}</h4>
      <div v-for="(value, index) in model.description" :key="index" v-show="this.isShow && !!value" class="description-list">
        <li>{{value}}</li>
        <button @click="deleteDescription(index, model.description)" class="delete-item">Удалить</button>
      </div>
      <input v-on:keyup.enter="onAddDescription" v-model="description" placeholder="Description" type="text">
      <button @click="onAddDescription" class="delete-item">Добавить</button>
      <div v-if="this.isShow">
        <button @click="changeShow" class="show-more">Скрыть список</button>
      </div>
      <div v-else>
        <button @click="changeShow" class="show-more">Показать список</button>
      </div>
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
        status: false
      }
    }
  },
  setup(props, { emit }) {
    const description = ref('')
    const emitOnDone = () => {
      emit('onDone')
    }
    const emitOnRemove = () => {
      emit('onRemove')
    }

    const onAddDescription = () => {
      emit('onAddDescription', {description: description.value});
      description.value = '';
    }
    const deleteDescription = (id, arr) => {
      delete arr[id];
    }

    return {
      description,
      emitOnDone,
      emitOnRemove,
      onAddDescription,
      deleteDescription
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
li{
  width: 200px;
}
.task-card {
  display: flex;
  justify-content: space-between;
}
.description-list{
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}
.delete-item{
  margin-left: 30px;
}
.show-more{
  margin-top: 15px;
}
</style>