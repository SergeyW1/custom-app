<template>
  <div>
    <div class="user-item" @click="openCardUser">
      <img src="../assets/user.svg" alt="icon-user" width="30" height="30" />
      <div class="user-item__inside">
        <div><strong>Пользователь: </strong>{{ user.firstName }}</div>
      </div>
    </div>
    <div class="not-active" :class="{ 'active-user': isActive }">
      <div class="user">
        <div>
          <div class="edit">
            <strong>Имя:</strong>
            <span v-if="!inputActive">{{ user.firstName }}</span>
            <form v-else @submit.prevent>
              <input
                class="user-data"
                type="text"
                @input="$emit('update:firstName', $event.target.value)"
                :value="user.firstName"
              />
            </form>
          </div>
          <div class="edit">
            <strong>Фамилия:</strong>
            <span v-if="!inputActive">{{ user.lastName }}</span>
            <form v-else @submit.prevent>
              <input
                class="user-data"
                type="text"
                @input="$emit('update:lastName', $event.target.value)"
                :value="user.lastName"
              />
            </form>
          </div>
          <div class="edit">
            <strong>Отчество:</strong>
            <span v-if="!inputActive">{{ user.patronymic }}</span>
            <form v-else @submit.prevent>
              <input
                class="user-data"
                type="text"
                @input="$emit('update:patronymic', $event.target.value)"
                :value="user.patronymic"
              />
            </form>
          </div>
          <div class="edit">
            <strong>Почта:</strong>
            <span v-if="!inputActive">{{ user.email }}</span>
            <form v-else @submit.prevent>
              <input
                class="user-data"
                type="text"
                @input="$emit('update:email', $event.target.value)"
                :value="user.email"
              />
            </form>
          </div>
        </div>
        <div class="btns">
          <button v-if="!inputActive" class="btn" @click="checkActive">
            <span>Изменить пользователя</span>
          </button>
          <button v-else class="btn" @click="changeUser">
            <span>Применить изменения</span>
          </button>
          <button class="btn" @click="$emit('modal-open', true, user)">
            Поиск новых пользователей
          </button>
          <button class="btn" @click="$emit('delete-user', user)">
            Удалить пользователя
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "user-item",
  data() {
    return {
      firstName: this.user.firstName,
      patronymic: this.user.patronymic,
      email: this.user.email,
      visibleModal: false,
      isActive: false,
      inputActive: false,
    };
  },
  props: {
    modelValue: [String, Number],
    user: {
      type: Object,
      required: true,
    },
  },
  methods: {
    checkActive() {
      this.inputActive = true;
    },
    changeUser() {
      this.inputActive = false;
      this.$emit("change-user", this.user);
    },
    modalOpen() {
      this.visibleModal = true;
    },
    modalClose() {
      this.visibleModal = false;
    },
    openCardUser() {
      this.isActive === true ? (this.isActive = false) : (this.isActive = true);
    },
  },
};
</script>

<style scoped>
.dialog {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  transition: all 0.3s ease;
  opacity: 0;
  visibility: hidden;
}

.dialog__content {
  margin: auto;
  background: white;
  border-radius: 12px;
  min-height: 50px;
  min-width: 300px;
  padding: 20px;
}

.dialog-active {
  opacity: 1;
  visibility: visible;
  transition: opacity 0.5s ease;
}
.not-active {
  position: fixed;
  visibility: hidden;
  display: flex;
  flex-direction: column;
  opacity: 0;
  margin: 20px 0;
  border: 2px solid rgb(81, 14, 174);
  padding: 20px;
  transition: all 0.3s ease;
  background-color: antiquewhite;
}
.active-user {
  z-index: 1;
  position: fixed;
  visibility: visible;
  opacity: 1;
}
.user-item {
  padding: 10px;
  user-select: none;
  border: 2px solid rgb(81, 14, 174);
  height: 50px;
  display: inline-flex;
  align-items: center;
  cursor: pointer;
}

.user-item__inside {
  font-size: 14px;
  font-weight: 500;
}

.icon-user {
  background: image("../");
}
.user {
  display: flex;
  flex-direction: column;
}
.user-data {
  display: block;
  padding: 9px 10px;
  border: 1px solid #0000001f;
  width: 200px;
  margin: 0 10px;
  height: 4px;
  background-color: antiquewhite;
  font-family: inherit;
  font-size: inherit;
  outline: none;
}

.edit {
  padding: 8px 0;
  display: flex;
  align-items: center;
}

.edit span {
  margin-left: 10px;
}

.btns {
  display: flex;
  justify-content: space-around;
  width: 586px;
}
.btn {
  font-size: 13px;
  font-weight: 600;
  margin-top: 10px;
  padding: 7px 10px;
  background: #fff;
  border: 2px solid rgb(81, 14, 174);
  cursor: pointer;
  color: blueviolet;
  background-color: antiquewhite;
}
</style>