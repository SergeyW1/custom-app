<template>
  <div>
    <div v-if="users.length > 0">
      <h2 class="list-active">Список пользователей</h2>
      <div v-for="user in users" :key="user.id">
        <user-item
          class="users"
          :user="user"
          users="users"
          v-model:first-name="user.firstName"
          v-model:last-name="user.lastName"
          v-model:patronymic="user.patronymic"
          v-model:email="user.email"
          @delete-user="$emit('delete-user', user)"
          @change-user="changeUser"
          @modal-open="$emit('modal-open', true, user)"
        />
      </div>
    </div>
    <div v-else>
      <h2 class="list-empty">Список пользователей пуст!</h2>
    </div>
  </div>
</template>

<script>
import UserItem from "./UserItem.vue";
export default {
  components: { UserItem },
  name: "users-item",
  props: {
    users: {
      type: Array,
      required: true,
    },
  },
  methods: {
    changeUser() {
      this.$emit("change-user", this.users);
    },
    openCardUser(user) {
      const userId = user.id;
      this.$emit("user-id", userId);
    },
  },
};
</script>

<style scoped>
.users {
  max-width: 300px;
  position: relative;
  margin: 20px 0;
}
.list-active {
  margin: 10px 0;
}
.list-empty {
  margin: 10px 0;
  color: red;
}
</style>