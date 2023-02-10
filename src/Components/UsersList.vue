<template>
  <div>
    <div class="user-name">
      <img src="../assets/user.svg" alt="icon-user" width="30" height="30" />
      <strong>Пользователь: </strong>{{ user.firstName }}
    </div>
    <div>
      <button
        class="btn-add-friend"
        @click="addFriend(user)"
        v-if="validFriend(user)"
      >
        Добавить в друзья
      </button>
      <button class="btn-add-friend" @click="deleteFriend(user)" v-else>
        Удалить из друзей
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "users-list",
  props: {
    user: {
      type: Object,
      required: true,
    },
    currentUser: {
      type: Object,
      required: true,
    },
  },
  methods: {
    addFriend(userAdd) {
      const addUsers = [];
      addUsers.push(userAdd.id);
      this.$emit("add-friend", addUsers);
    },
    deleteFriend(userAdd) {
      this.$emit("delete-friend", userAdd);
    },
    validFriend(user) {
      return (
        this.currentUser.friends.find((item) => item.id === user.id) ===
        undefined
      );
    },
  },
};
</script>

<style scoped>
.user-name {
  display: flex;
  margin-right: 75px;
  align-items: center;
}

.btn-add-friend {
  font-size: 13px;
  font-weight: 600;
  padding: 7px 10px;
  background: #fff;
  border: 2px solid rgb(81, 14, 174);
  cursor: pointer;
  color: blueviolet;
}
</style>