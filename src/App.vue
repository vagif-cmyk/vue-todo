<script>
let id = 0; // айдишник для элементов в списке

export default {
  data() {
    return {
      // массив тоdos в кот. захардкодены данные, в реальном приложении будет по умолчанию пустым
      todos: [
        {
          title: "Погладить кота и жену",
          done: false,
          id: id++,
        },
        {
          title: "Признаться сыну что он приемный",
          done: true,
          id: id++,
        },
        {
          title: "Убедить соседку что ты реально пришел за солью",
          done: false,
          id: id++,
        },
      ],
      text: "", // хранится значения в главном инпуте
      valueModalInput: "", // хранится значения в инпуте модалки изменения дела
      openChangeModal: false, // флаг при помощи кот. происходит управление открыть/закрыть модалку редактирования
      currentTodo: null, // при клике на редактировать хранит объект с текущим делом
    };
  },
  methods: {
    // добавляет в массив дел новое дело
    onCreateTodo() {
      if (this.text.trim()) {
        // проверяем не пустое ли значение
        this.todos.push({ title: this.text.trim(), done: false, id: id++ });
        this.text = "";
      }
    },
    // удаляет дело из массива дел
    onDeleteTodo(todo) {
      this.todos = this.todos.filter((item) => item !== todo);
    },
    // открывает модалку, записывает текущий объект в поле  currentTodo, устанавливает в value инпута строку с описанием текущего дела
    onChangeTodo(todo) {
      this.currentTodo = todo;
      this.openChangeModal = true;
      this.valueModalInput = todo.title;
    },
    // сохраняет новое описание дела в текущее дело
    onSave() {
      if (!this.valueModalInput.trim()) {  //  если значения пустое, выходим из функции
        this.openChangeModal = false;
        return;
      }
      this.currentTodo.title = this.valueModalInput.trim();
      this.openChangeModal = false;
      this.valueModalInput = "";
    },
    // закрывает модалку
    onCancel() {
      this.openChangeModal = false;
      this.valueModalInput = "";
    },
  },
};
</script>

<template>
  <div class="container">
    <form>
      <input
        v-model="text"
        type="text"
        placeholder="Введите описание дела"
        class="formInput"
      />
      <button @click.prevent="onCreateTodo" class="formBtn">
        Создать дело
      </button>
    </form>
    <div v-if="todos.length === 0">Начните создавать дела!</div>
    <ul v-else>
      <li
        v-for="todo in todos"
        :class="{ done: todo.done }"
        :key="todo.id"
        class="item"
      >
        <div class="check">
          <input v-model="todo.done" type="checkbox" class="checkbox" />
          <h2>{{ todo.title }}</h2>
        </div>
        <div class="btns">
          <button @click="onDeleteTodo(todo)" class="btnDelete">удалить</button>
          <button @click="onChangeTodo(todo)" class="btnChange">
            редактировать
          </button>
        </div>
      </li>
    </ul>
  </div>
  <div v-if="openChangeModal" class="wrapper">
    <div class="modalChange">
      <h2 class="modalTitle">Редактирование</h2>
      <input
        v-model="valueModalInput"
        type="text"
        placeholder="Введите дело"
        class="modalInput"
      />
      <button @click="onSave" class="save">Сохранить</button>
      <button @click="onCancel" class="cancel">x</button>
    </div>
  </div>
</template>

<style scoped>
  
.container {
  width: 100%;
  padding: 40px 140px 100px 40px;
  background-color: #75f09291;
  border-radius: 10px;
}
.check,
.btns {
  display: flex;
}
.btnChange,
.btnDelete {
  padding: 4px 8px;
  border: none;
  border-radius: 4px;
  background-color: rgba(255, 255, 255, 0.619);
  cursor: pointer;
}
.btnDelete {
  margin-right: 10px;
  transition: all 0.2s ease;
}
.btnDelete:hover {
  color: #fff;
  background-color: red;
}
.checkbox {
  margin-right: 10px;
  width: 20px;
}
.check {
  margin-right: 80px;
}
.item {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
  padding: 10px;
  border-radius: 4px;
  border-bottom: 1px solid #000;
  list-style-type: none;
}
.form {
  display: flex;
  flex-direction: column;
}
.formInput {
  width: 100%;
  padding: 8px 4px;
  margin-bottom: 10px;
  border: none;
  border-radius: 4px;
  background-color: rgba(255, 255, 255, 0.807);
  font-size: 1.1em;
  font-style: italic;
  transition: all 0.1s ease;
}
.formInput::placeholder {
  font-style: normal;
  font-size: 14px;
}
.formInput:focus {
  outline: none;
  box-shadow: 0 0 10px 4px rgba(50, 36, 247, 0.518);
}
.formBtn {
  padding: 8px 6px;
  margin-bottom: 20px;
  border: none;
  border-radius: 4px;
  color: #fff;
  background-color: rgba(6, 123, 240, 0.793);
}
.done {
  background-color: #04730496;
}
.wrapper {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgb(55 55 55 / 79%);
}
.modalChange {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 50%;
  height: 50%;
  padding: 40px;
  border-radius: 10px;
  background-color: rgba(50, 36, 247, 0.518);
}
.modalTitle {
  margin-bottom: 30px;
  font-size: 1.6em;
  color: rgb(251, 233, 233);
}
.modalInput {
  width: 100%;
  margin-bottom: 20px;
  padding: 5px;
  border: none;
  border-radius: 10px;
  font-size: 1.1em;
}
.cancel {
  position: absolute;
  right: 10px;
  top: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 20px;
  height: 20px;
  padding: 4px;
  border: none;
  border-radius: 50%;
  font-size: 1.1em;
  cursor: pointer;
}
.save {
  padding: 5px 10px;
  border: none;
  border-radius: 10px;
  font-size: 1.1em;
  background-color: rgba(6, 123, 240, 0.793);
  color: #fff;
}


</style>
