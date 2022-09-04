<script>
export default {
  data() {
    return {
      value: this.todo.title,
      closeModal: this.close
    };
  },
  props: {
    todo: Object,
  },
  methods: {
    onSave() {
      this.todo.title = this.value;
      console.log(this.todo);
    },
  },
};
</script>

<template>
  <div class="wrapper">
    <div class="modal">
      <h3 class="title">Редактировать дело</h3>
      <input  v-model="value" type="text" placeholder="Введите дело" />
      <button @click="onSave">Сохранить</button>
    </div>
  </div>
</template>
<style scoped>
.wrapper {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(50, 49, 49, 0.85);
}
.modal {
  padding: 40px;
  border-radius: 10px;
  background-color: rgb(75, 171, 163);
}
.title {
  margin-bottom: 10px;
}
</style>
