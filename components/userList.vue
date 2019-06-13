<template>
  <v-container class="users">
    <ul class="userList">
      <li v-for="(user, index) of userData" :key="index" class="userList__element">
        <v-layout pa-1 wrap align-start>
          <v-flex
            class="userList__element--block userList__element--idBox"
            xs12
            sm4
            md2
          >User ID: {{ user.id }}</v-flex>
          <v-flex class="userList__element--block" xs12 sm4 md2>
            Username:
            <div v-if="editState === index">
              <input v-model="provUserData.name" class="userList__element--editable">
            </div>
            <div v-else>{{user.name}}</div>
          </v-flex>
          <v-flex class="userList__element--block" xs12 sm6 md3>
            Email:
            <div v-if="editState === index">
              <input v-model="provUserData.email" class="userList__element--editable">
            </div>
            <div v-else>{{user.email}}</div>
          </v-flex>
          <v-flex class="userList__element--block" xs12 sm6 md3>
            Company:
            <div v-if="editState === index">
              <input v-model="provUserData.company" class="userList__element--editable">
            </div>
            <div v-else>{{user.company.name}}</div>
          </v-flex>
          <v-flex class="userList__element--block" xs12 sm4 md2>
            <v-btn
              outline
              block
              class="editButton"
              v-if="editState === index"
              @click="saveUser(index)"
            >Save User</v-btn>
            <v-btn outline block class="editButton" v-else @click="editUser(index)">Edit User</v-btn>
          </v-flex>
        </v-layout>
      </li>
    </ul>
  </v-container>
</template>
<script>
import axios from "axios";
export default {
  data() {
    let userData;
    let editState = null;
    let provUserData = {
      name: "",
      email: "",
      company: ""
    };
    return {
      userData,
      editState,
      provUserData
    };
  },
  methods: {
    editUser(i) {
      this.editState = i;
      let provUser = this.userData[i];
      this.provUserData.name = this.userData[i].name;
      this.provUserData.email = this.userData[i].email;
      this.provUserData.company = this.userData[i].company.name;
    },
    saveUser(i) {
      this.editState = null;
      this.userData[i].name = this.provUserData.name;
      this.userData[i].email = this.provUserData.email;
      this.userData[i].company.name = this.provUserData.company;
      axios
        .put("https://jsonplaceholder.typicode.com/users", this.userData[i])
        .then(response => (this.userData = response.data))
        .catch(error => `an ${error} has occurred`);
    }
  },
  mounted() {
    axios
      .get("https://jsonplaceholder.typicode.com/users")
      .then(response => (this.userData = response.data))
      .catch(error => `no information retrieved because of error ${error}`);
  }
};
</script>
<style lang="scss">
.users,
.userList,
.userList__element,
.userList__element--block {
  box-sizing: border-box;
}
.users {
  padding: 15px;
}
.userList {
  list-style-type: none;
  padding-left: 0;
}
.userList__element {
  margin: 5px 0;
  width: 100%;
  &--block {
    padding: 5px;
  }
  &--idBox {
    line-height: 45px;
  }
  &--editable {
    border: 1px solid #c1c1c1;
    border-radius: 4px;
    padding: 0 5px;
  }
}
.editButton {
  border-radius: 4px;
}
/* overwrrides */
.layout.column > .flex {
  width: 100%;
}
</style>