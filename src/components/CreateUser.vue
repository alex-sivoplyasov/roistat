<template>
  <div class="modal">
    <div class="modal__container">
      <div class="modal__header">
        <div class="modal__title">
          Добавления пользователя
        </div>

        <div class="modal__close" @click="closeForm">Закрыть</div>
      </div>
      <div class="modal__content">
        <form @submit.prevent="onSubmit" class="modal__form">
          <div class="modal__field-block">
            <label for="user-name" class="modal__field-name">Имя</label>
            <input type="text" name="name" v-model="name" id="user-name" class="modal__field-value">
          </div>

          <div class="modal__field-block">
            <label for="user-phone" class="modal__field-name">Телефон</label>
            <input type="text" name="phone" v-model="phone" id="user-phone" class="modal__field-value">
          </div>

          <div class="modal__field-block" v-if="users.length">
            <div class="modal__field-name">Начальник</div>
            <select name="user-director" v-model="parent" class="modal__field-value">
              <option :value="user.id" v-for="user in users"> {{user.name}}</option>
            </select>
          </div>


          <div class="modal__error" v-if="error">
            Заполните поля
          </div>

          <button type="submit" class="button">Сохранить</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
name: "CreateUser",
  props: ['users'],
  data: () => ({
    name: null,
    phone: null,
    parent: null,
    error: false
  }),
  methods: {
    onSubmit() {
      if (this.name && this.phone) {
        const user = {
          id: Date.now(),
          name: this.name,
          phone: this.phone,
          parent: this.parent
        }
        this.$emit('add', user)
      } else {
        this.error= true
      }
    },
    closeForm() {
      this.$emit('close')
    }
  }
}
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100vw;
  height: 100vh;
  background-color: rgba(255, 255, 255, 0.9);
}

.modal__container {
  width: 600px;
  border: 1px solid #000000;
  background: #FFFFFF;
  padding: 16px;
}

.modal__header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 32px;
}

.modal__close {
  cursor: pointer;
}

.modal__field-block {
  display: flex;
  margin-bottom: 16px;
}

.modal__field-name {
  width: 30%;
}

.modal__error {
  margin-bottom: 16px;
}
</style>
