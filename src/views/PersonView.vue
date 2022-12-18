<script setup lang="ts">
import { isTemplateElement } from "@babel/types";
import { ref, reactive, watch, nextTick } from "vue";
class Person {
  id: number;
  name: string;
  surname: string;
  gender: string;
  static lastId = 1;
  constructor(name: string, surname: string, gender: string) {
    this.id = Person.lastId++;
    this.name = name;
    this.surname = surname;
    this.gender = gender;
  }
}
const person = reactive<Person>({ id: -1, name: "", surname: "", gender: "" });
const msg = reactive({ name: "", surname: "", gender: "" });
const personList = ref<Person[]>([]); //Array
const showForm = ref(false);
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

  nextTick(() => {
    msg.name = "";
    msg.surname = "";
    msg.gender = "";
  });
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
    if (person.id < 0) {
      // Add new
      const p = new Person(person.name, person.surname, person.gender);
      personList.value.push(p); //push to array
    } else {
      // Edit
      const index = personList.value.findIndex((item) => item.id === person.id);
      personList.value[index].name = person.name;
      personList.value[index].surname = person.surname;
      personList.value[index].gender = person.gender;
    }

    clearForm();
    showForm.value = false;
  }
}
const doEdit = function (p: Person) {
  person.name = p.name;
  person.surname = p.surname;
  person.gender = p.gender;
  person.id = p.id;
  showForm.value = true;
};
</script>
<template>
  <div v-if="showForm">
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
  </div>
  <div v-if="!showForm">
    <div style="width: 100%; text-align: right">
      <button style="width: 100px" @click="showForm = !showForm">Add</button>
    </div>
    <table id="persons">
      <thead>
        <th>ID</th>
        <th>First Name</th>
        <th>Surname</th>
        <th>Gender</th>
        <th style="width: 250px">Operation</th>
      </thead>
      <tr v-for="(item, index) in personList" :key="index">
        <td>{{ item.id }}</td>
        <td>{{ item.name }}</td>
        <td>{{ item.surname }}</td>
        <td>{{ item.gender }}</td>
        <td>
          <button
            style="width: 100px; background-color: darkcyan"
            @click="doEdit(item)"
          >
            Edit</button
          ><button
            style="width: 100px; margin-left: 1em; background-color: red"
          >
            Delete
          </button>
        </td>
      </tr>
      <tr>
        <td
          colspan="4"
          style="text-align: center"
          v-if="personList.length === 0"
        >
          No Data
        </td>
      </tr>
    </table>
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

input[type="submit"],
button {
  width: 100%;
  background-color: #0787f7;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

input[type="submit"]:hover {
  background-color: #0787f7;
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

#persons {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

#persons td,
#persons th {
  border: 1px solid #ddd;
  padding: 8px;
}

#persons tr:nth-child(even) {
  background-color: #f2f2f2;
}

#persons tr:hover {
  background-color: #ddd;
}

#persons th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #04aa6d;
  color: white;
}
</style>
