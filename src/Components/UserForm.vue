<template>
  <div>
    <div class="app__btns">
      <button class="btn" @click="updateModal">Добавить пользователя</button>
      <button class="btn" @click="modalConnectUser">
        Показать связь пользователей
      </button>
    </div>
    <div
      class="dialog"
      :class="{ 'dialog-active': visibleModalConnect }"
      @click.stop="closeModalConnectUser"
    >
      <div @click.stop class="dialog__content">
        <div class="userList-header">
          <div>Связи с другими пользователями</div>
          <button @click="closeModalConnectUser" class="btn-close"></button>
        </div>
        <div class="connect-userList">
          <div
            v-for="user in users"
            :key="user.id"
            class="connect-userList__user"
          >
            <user-connections :user="user" />
          </div>
        </div>
      </div>
    </div>
    <div
      class="dialog"
      :class="{ 'dialog-active': visibleModal }"
      @click.stop="modalClose"
    >
      <div @click.stop class="dialog__content">
        <div class="dialog__content-header">
          <div>Новый Пользователь</div>
          <button @click="modalClose" class="btn-close"></button>
        </div>
        <form @submit.prevent class="form-user">
          <input class="input" v-model="firstName" placeholder="Имя" />
          <input class="input" v-model="lastName" placeholder="Фамилия" />
          <input class="input" v-model="patronymic" placeholder="Отчество" />
          <input class="input" v-model="email" placeholder="Почта" />
          <div class="btn-content">
            <button class="btn" @click="addNewUser">
              Создать пользователя
            </button>
          </div>
        </form>
      </div>
    </div>
    <users-item
      :users="users"
      @delete-user="deleteUser"
      @change-user="userFieldChanges"
      @modal-open="openModalUser"
      v-model:first-name="firstName"
      v-model:last-name="lastName"
      v-model:patronymic="patronymic"
      v-model:email="email"
    />
    <div
      class="dialog"
      :class="{ 'dialog-active': visibleModalUser }"
      @click.stop="closeModalUser"
    >
      <div @click.stop class="dialog-user">
        <div>
          <input
            type="text"
            placeholder="Поиск новых друзей"
            class="input"
            v-model="searchUser"
          />
        </div>
        <div class="users">
          <div v-for="user in filterSearch" :key="user.id">
            <users-list
              :user="user"
              :currentUser="currentUser"
              class="user-list"
              @add-friend="addFriend(user)"
              @delete-friend="deleteFriend(user)"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { v4 as uuidv4 } from "uuid";
import UsersItem from "./UsersItem.vue";
import UsersList from "./UsersList.vue";
import UserConnections from "./UserConnections.vue";
export default {
  components: { UsersItem, UsersList, UserConnections },
  name: "user-form",
  data() {
    return {
      users: [
        {
          id: uuidv4(),
          firstName: "John",
          lastName: "Smith",
          patronymic: "Vlahovic",
          email: "vsehorosho@mail.ru",
          friends: [],
        },
        {
          id: uuidv4(),
          firstName: "Severus",
          lastName: "Snape",
          patronymic: "Snapepovich",
          email: "avadakidabra@mail.ru",
          friends: [],
        },
      ],
      connectUserModal: false,
      userId: [],
      userPackage: [],
      searchUser: "",
      firstName: "",
      lastName: "",
      patronymic: "",
      email: "",
      countFrient: "",
      visibleModal: false,
      visibleModalUser: false,
      visibleModalConnect: false,
      currentUser: {},
    };
  },
  methods: {
    addNewUser() {
      if (
        this.firstName !== "" &&
        this.lastName !== "" &&
        this.patronymic !== "" &&
        this.email !== ""
      ) {
        this.users.push({
          id: uuidv4(),
          firstName: this.firstName,
          lastName: this.lastName,
          patronymic: this.patronymic,
          email: this.email,
          friends: [],
        });
        this.firstName = "";
        this.lastName = "";
        this.patronymic = "";
        this.email = "";
        this.modalClose();
        const usersId = [];
        this.users.forEach((user) => {
          usersId.push(user.id);
        });
        this.userId = [...new Set(usersId)];
        localStorage.setItem("users", JSON.stringify(this.users));
        localStorage.setItem("userId", JSON.stringify(this.userId));
      }
    },
    userFieldChanges(users) {
      localStorage.setItem("users", JSON.stringify(users));
    },
    updateModal() {
      this.visibleModal = true;
    },
    modalClose() {
      this.visibleModal = false;
    },
    modalConnectUser() {
      this.visibleModalConnect = true;
    },
    closeModalConnectUser() {
      this.visibleModalConnect = false;
    },
    deleteUser(user) {
      this.users = this.users.filter((item) => item.id !== user.id);
      this.users = this.users.filter(
        (item) =>
          (item.friends = item.friends.filter((items) => items.id !== user.id))
      );
      this.userId = this.userId.filter((item) => item !== user.id);
      localStorage.setItem("users", JSON.stringify(this.users));
      localStorage.setItem("userId", JSON.stringify(this.userId));
    },
    openModalUser(bool, user) {
      this.currentUser = user;
      this.userPackage = this.users.filter((i) => i.id !== user.id);
      this.visibleModalUser = bool;
    },
    closeModalUser() {
      this.visibleModalUser = false;
    },
    addFriend(arr) {
      console.log(arr.id);
      const objUser = { id: arr.id, userName: arr.firstName };
      this.currentUser.friends.push(objUser);
      localStorage.setItem("users", JSON.stringify(this.users));
    },
    deleteFriend(userAdd) {
      this.currentUser.friends = this.currentUser.friends.filter(
        (item) => item.id !== userAdd.id
      );
      localStorage.setItem("users", JSON.stringify(this.users));
    },
  },
  computed: {
    filterSearch() {
      return this.userPackage.filter((user) => {
        return user.firstName
          .toLowerCase()
          .includes(this.searchUser.toLowerCase());
      });
    },
  },
  mounted() {
    const dataUserId = localStorage.getItem("userId", this.userId);
    const data = localStorage.getItem("users", this.users);
    if (data) {
      this.userId = JSON.parse(dataUserId);
      this.users = JSON.parse(data);
    } else {
      localStorage.setItem("users", JSON.stringify(this.users));
    }
  },
};
</script>

<style scoped>
.form-user {
  display: flex;
  flex-direction: column;
  margin: 26px 0;
  padding: 0 30px;
}
.btn {
  font-size: 13px;
  margin-top: 21px;
  font-weight: 600;
  padding: 7px 10px;
  background: #fff;
  border: 2px solid rgb(81, 14, 174);
  cursor: pointer;
  color: blueviolet;
}

.btn-content {
  display: flex;
  justify-content: flex-end;
}

.dialog__content-header {
  display: flex;
  border-bottom: 2px solid #00000017;
  justify-content: space-around;
  padding: 15px 0;
  width: 100%;
  background: #b7b7b726;
  border-top-left-radius: 12px;
  border-top-right-radius: 12px;
}

.btn-close {
  cursor: pointer;
  border: none;
  height: 21px;
  width: 21px;
  display: flex;
  position: relative;
}

.btn-close::after {
  content: "";
  position: absolute;
  width: 25px;
  height: 2px;
  border-radius: 40px;
  background: rgba(123, 122, 122, 0.73);
  top: 10px;
  right: -3px;
  transform: rotate(45deg);
}

.btn-close::before {
  content: "";
  position: absolute;
  width: 25px;
  height: 2px;
  border-radius: 40px;
  background: rgba(123, 122, 122, 0.73);
  top: 10px;
  right: -3px;
  transform: rotate(-45deg);
}

.btn-close:hover::before,
.btn-close:hover::after {
  background: rgb(101, 100, 100);
}

.input {
  width: 100%;
  border: 2px solid rgb(81, 14, 174);
  padding: 10px 15px;
  margin-top: 15px;
  outline: none;
  font-family: "Comfortaa", cursive;
}
.app__btns {
  display: flex;
  justify-content: space-between;
}
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
}

.dialog-active {
  opacity: 1;
  visibility: visible;
  transition: opacity 0.5s ease;
}

.userList-header {
  display: flex;
  border-bottom: 2px solid #00000017;
  justify-content: space-between;
  padding: 15px 20px;
  background: #b7b7b726;
  border-top-left-radius: 12px;
  border-top-right-radius: 12px;
  font-size: 19px;
}

.users {
  margin-top: 50px;
}

.dialog-user {
  margin: auto;
  background: white;
  border-radius: 12px;
  min-height: 400px;
  min-width: 450px;
  padding: 20px;
}

.user-list {
  font-size: 14px;
  border: 2px solid rgb(81, 14, 174);
  margin: 10px 0;
  padding: 10px 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.connect-userList {
  padding: 20px;
  width: 500px;
}

.connect-userList__user {
  margin: 20px 0;
}
</style>

