<script setup lang="ts">
import { reactive } from "vue";
const person = reactive({ name: "", surname: "", gender: "" });
function dosubmit() {
  console.log(person);
}
</script>
<template>
  <div>
    <form>
      <label for="name">First Name</label>
      <input type="text" id="name" v-model="person.name" autocomplete="off" />
      <label for="surname">Surname</label>
      <input
        type="text"
        id="surname"
        v-model="person.surname"
        autocomplete="off"
      />

      <label for="gender">Gender</label>
      <select id="gender" v-model="person.gender">
        <option hidden selected>Select Gender</option>
        <option value="M">Male</option>
        <option value="F">Female</option>
      </select>
      <input type="submit" value="Submit" @click.prevent="dosubmit" />
    </form>
    <pre>{{ person }}</pre>
  </div>
</template>

<style scoped>
input[type="text"],
select {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

input[type="submit"] {
  width: 100%;
  background-color: #cd51ac;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

input[type="submit"]:hover {
  background-color: #cd51ac;
}

div {
  border-radius: 5px;
  background-color: #f2f2f2;
  padding: 20px;
}
</style>
