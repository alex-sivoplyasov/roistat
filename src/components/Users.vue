<template>
  <div class="users">
    <div class="users__list" v-if="users.length">
      <div class="users__item">
        <div class="users__name users__field head" @click="sort('name')">Имя</div>
        <div class="users__phone users__field head" @click="sort('phone')">Телефон</div>
      </div>
      <div class="users__item" v-for="user in users" :key="user.id">
        <div class="users__name users__field"> {{ user.name }}</div>
        <div class="users__phone users__field">{{ user.phone }}</div>

        <div class="users__list" v-if="user.children">
          <div class="users__item" v-for="child in user.children" :key="child.id">
            <div class="users__name users__field"> -{{ child.name }}</div>
            <div class="users__phone users__field"> {{ child.phone }}</div>

            <div class="users__list" v-if="child.children">
              <div class="users__item" v-for="child1 in child.children" :key="child1.id">
                <div class="users__name users__field"> --{{ child1.name }}</div>
                <div class="users__phone users__field"> {{ child1.phone }}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Users",
  props: ['users'],
  methods: {
    sort(field) {
      this.$emit('sort', field)
    }
  }
}
</script>

<style scoped>
div {
  box-sizing: border-box;
}

.users__list {
  margin-top: 32px;
}

.users__item {
  display: flex;
  flex-wrap: wrap;
}

.users__field {
  width: 50%;
  padding: 8px 16px;
  box-shadow: 1px 0 0 0 #000000,
  0 1px 0 0 #000000,
  1px 1px 0 0 #000000,
  1px 0 0 0 #000000 inset,
  0 1px 0 0 #000000 inset;
}

.users__field.head {
  cursor: pointer;
  background: aquamarine;
}

.users__item .users__list {
  margin-top: 0;
  margin-left: 20px;
  width: calc(100% - 20px);
}

.users__item .users__list .users__item {
  width: 100%;
}
</style>
