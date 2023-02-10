<template>
  <div>
    <div><strong>Пользователь: </strong> {{ user.firstName }}</div>
    <div v-if="checkValidFrinds(user)">
      <div v-for="item in user.friends" :key="item.id">
        {{ user.firstName }} добавил пользователя ({{ item.userName }})
      </div>
    </div>
    <div v-else>Список друзей пуст!</div>
  </div>
</template>

<script>
export default {
  name: "user-connections",
  props: {
    user: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      friendsList: "",
      checkingFriends: false,
    };
  },
  methods: {
    checkValidFrinds(user) {
      return user.friends.find((item) => item) !== undefined;
    },
  },
};
</script>

<style scoped>
</style>