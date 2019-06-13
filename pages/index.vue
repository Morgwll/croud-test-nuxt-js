<template>
  <v-layout column justify-center align-center>
    <v-flex xs6 sm3 md3>
      <v-card>
        <v-card-title class="headline">Modify the information</v-card-title>
        <user-list/>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import userList from "../components/userList";
export default {
  components: {
    userList
  }
};
</script>
