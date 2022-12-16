<script setup lang="ts">
import { ref, reactive, watch } from "vue";
const person = reactive({ name: "", surname: "", gender: "" });
const msg = reactive({ name: "", surname: "", gender: "" });

const checkName = function (name: string) {
  if (name.trim().length == 0) {
    msg.name = "First name is empty !";
    return false;
  }
  msg.name = "";
  return true;
};

const checkSurname = (surname: string) => {
  if (surname.trim().length == 0) {
    msg.surname = "Surname is empty !";
    return false;
  }
  msg.surname = "";
  return true;
};

function checkGender(gender: string) {
  if (gender.trim().length == 0) {
    msg.gender = "Please choose your gender.";
    return false;
  }

  msg.gender = "";
  return true;
}

const clearForm = function () {
  person.name = "";
  person.surname = "";
  person.gender = "";
  msg.name = "";
  msg.surname = "";
  msg.gender = "";
};

watch(
  () => person.name,
  (name) => {
    checkName(name);
  }
);

watch(
  () => person.surname,
  (surname) => {
    checkSurname(surname);
  }
);

watch(
  () => person.gender,
  (gender) => {
    checkGender(gender);
  }
);

function doSubmit() {
  if (
    checkName(person.name) &&
    checkSurname(person.surname) &&
    checkGender(person.gender)
  ) {
    console.log(person);
    clearForm();
  }
}
</script>
<template>
  <div>
    <form>
      <label for="name">First Name</label>
      <input type="text" id="name" v-model="person.name" autocomplete="off" />
      <span class="error">{{ msg.name }}</span>

      <label for="surname">Surname</label>
      <input
        type="text"
        id="surname"
        v-model="person.surname"
        autocomplete="off"
      />
      <span class="error">{{ msg.surname }}</span>

      <label for="gender">Gender</label>
      <select id="gender" v-model="person.gender">
        <option hidden selected>Select Gender</option>
        <option value="M">Male</option>
        <option value="F">Female</option>
      </select>
      <span class="error">{{ msg.gender }}</span>
      <input type="submit" value="Submit" @click.prevent="doSubmit" />
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

.error {
  color: red;
  font-size: smaller;
  display: block;
}
</style>
