<script setup>

import {reactive, ref} from "vue";

let userList = ref([]);

const initialState= {
  id: 0,
  name: '',
  email: '',
  role: '',
}
let user = reactive({...initialState});

let selectedUser = ref(null);

const selectUser = (row) => {
  selectedUser.value = row;
  user = {
    id: row.id,
    name: row.name,
    email: row.email,
    role: row.role,
  };
  console.table(selectedUser);
  console.table(user);
}


const removeUser = (row) => {
  if(confirm(`Are you sure you want to delete ${row.name}?`)) {
    let index = userList.value.findIndex((u) => u.id === row.id);
    index !== -1 && userList.value.splice(index,1);
  }
}

const addUser = () => {
  if (validateUser(user)) {
    let id = userList.value.length;
    userList.value.push({
      id: ++id,
      name: user.name,
      email: user.email,
      role: user.role
    })
    // Resets form
    Object.assign(user, {...initialState});
  }
}

const updateUser = () => {
    let index = userList.value.findIndex((u) => u.id === selectedUser.value.id);
    index !== -1 && (userList.value[index] = {
      name: user.name,
      email: user.email,
      role: user.role,
    });
    user.value = selectedUser.value = null;

}


const validateUser = (new_user) => {
  let index = userList.value.findIndex((u) => u.email === new_user.email);
  if (index !== -1) {
    alert('Email already exists!'); 
    return false;
  } else{
    return true;
  }
}

</script>

<template>
  <div class="container my-5">
    <div class="row mb-4">
      <div class="col mb-3">
        <div class="card">
          <div class="card-header">
            <h5 class="card-title">User Management</h5>
          </div>
          <div class="card-body">
            <form @submit.prevent="!selectedUser ? addUser() : updateUser()">
              <div class="mb-3">
                <label for="name" class="form-label">Name</label>
                <input v-model="user.name" type="text" class="form-control" id="name" placeholder="Enter name">
              </div>
              <div class="mb-3">
                <label for="email" class="form-label">Email</label>
                <input v-model="user.email" type="email" class="form-control" id="email" placeholder="Enter email">
              </div>
              <div class="mb-3">
                <label for="role" class="form-label">Role</label>
                <select v-model="user.role" class="form-select" id="role">
                  <option value="admin">Admin</option>
                  <option value="staff">Staff</option>
                </select>
              </div>
              <button type="submit" class="btn btn-primary">Save</button>
            </form>
          </div>
        </div>
      </div>

      <div class="col">
        <div class="card">
          <div class="card-header">
            <h5 class="card-title">Users</h5>
          </div>
          <div class="card-body">
            <div @click="selectUser(row)" :class="selectedUser && selectedUser.id === row.id ? 'card mt-2 border border-primary' : 'card mt-2'" v-for="row in userList" :key="row.id">
              <div class="card-body">
                <div class="d-flex justify-content-between align-items-center">
                  <div>
                    <h6 class="card-title mb-1">{{ row.name }}</h6>
                    <p class="card-text mb-0">{{ row.email }}</p>
                    <p class="card-text mb-0">{{ row.role }}</p>
                  </div>
                  <div class="d-flex gap-2">
                    <button @click.stop="removeUser(row)" class="btn btn-sm btn-danger">Delete</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<style scoped>

</style>
