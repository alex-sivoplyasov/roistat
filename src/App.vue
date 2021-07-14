<template>
  <div id="app">
    <div class="button" @click="openForm">Добавить</div>
    <Users :users="usersTree" @sort="sortTable"/>
    <CreateUser :users="users" @add="addUser" @close="closeForm" v-if="isFormOpen"/>
  </div>
</template>

<script>
import Users from './components/Users'
import CreateUser from './components/CreateUser'


export default {
  name: 'App',
  data: () => ({
    users: [],
    isFormOpen: false,
    currentSortDirection: 'asc'
  }),
  mounted() {
    const users = JSON.parse(localStorage.getItem('users'))
    if(users)
      this.users = users
  },
  computed: {
    usersTree() {
      return this.getUsersTree(this.users)
    }
  },
  methods: {
    addUser(user) {
      this.users.push(user)
      localStorage.setItem('users', JSON.stringify(this.users))
      this.closeForm()
    },
    closeForm() {
      this.isFormOpen = false
    },
    openForm() {
      this.isFormOpen = true
    },
    getUsersTree(items, id = null) {
      return items
        .filter(item => item['parent'] === id)
        .map(item => ({ ...item, children: this.getUsersTree(items, item.id) }));
    },
    sortTable(field) {
      this.users.sort(this.sortByField(field))
    },
    sortByField(field) {
      if (this.currentSortDirection === 'asc') {
        this.currentSortDirection = 'desc'
        return (a, b) => a[field] > b[field] ? 1 : -1
      } else {
        this.currentSortDirection = 'asc'
        return (a, b) => a[field] < b[field] ? 1 : -1
      }
    }
  },
  components: {
    Users, CreateUser
  }
}
</script>

<style>
#app {
  width: 600px;
  margin: 0 auto;
}

.button {
  width: fit-content;
  margin-left: auto;
  padding: 8px 16px;
  background: #FFFFFF;
  border-radius: 16px;
  border: 1px solid #000000;
  cursor: pointer;
}

.button:hover {
  background: #000000;
  color: #FFFFFF;
}

</style>
