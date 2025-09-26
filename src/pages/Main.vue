<script setup>
import { ref } from "vue";
import { PencilIcon, TrashIcon } from "@heroicons/vue/24/solid";
const fullName = ref("");
const email = ref("");
const password = ref("");

const users = ref([]);

const editing = ref(null);

const handleSubmit = () => {
  if (editing.value) {
    const inx = users.value.findIndex((user) => user.id === editing.value.id);

    if (inx < 0) {
      return;
    }

    users.value[inx] = {
      ...users.value[inx],
      fullName: fullName.value,
      email: email.value,
      password: password.value,
    };
    editing.value = null;
    fullName.value = "";
    email.value = "";
    password.value = "";
  } else {
    const newUser = {
      id: Date.now(),
      fullName: fullName.value,
      email: email.value,
      password: password.value,
    };
    users.value.push(newUser);
    fullName.value = "";
    email.value = "";
    password.value = "";
  }
};

const handleEditUser = (id) => {
  const existsUser = users.value.find((user) => user.id === id);

  if (!existsUser) {
    return;
  }
  fullName.value = existsUser.fullName;
  email.value = existsUser.email;
  password.value = existsUser.password;
  editing.value = existsUser;
};

const handleDelete = (id) => {
  users.value = users.value.filter((user) => user.id !== id);
};
</script>

<template>
  <div class="wrapper">
    <form class="form-input" @submit.prevent="handleSubmit">
      <h2>User registration</h2>
      <input v-model="fullName" type="text" placeholder="Full Name" />
      <input v-model="email" type="email" placeholder="Email Address" />
      <input v-model="password" type="password" placeholder="Password" />
      <button>{{ editing ? "Save" : "Submit" }}</button>
    </form>

    <div class="user-table">
      <h2>Users List</h2>
      <table>
        <thead>
          <tr>
            <th>#</th>
            <th>Full Name</th>
            <th>Email</th>
            <th>Password</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(user, inx) in users" :key="inx">
            <td>{{ inx + 1 }}</td>
            <td>{{ user.fullName }}</td>
            <td>{{ user.email }}</td>
            <td>{{ user.password }}</td>
            <td class="buttons">
              <PencilIcon @click="handleEditUser(user.id)" class="edit-icon" />
              <TrashIcon @click="handleDelete(user.id)" class="delete-icon" />
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  height: 898px;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  gap: 7rem;
  padding-top: 3rem;
  background: linear-gradient(135deg, #6366f1, #a855f7, #ec4899);
}

.form-input {
  background: #fff;
  padding: 1rem 3rem;
  border-radius: 1rem;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
  display: flex;
  flex-direction: column;
  gap: 1rem;
  width: 350px;
}

.form-input h2 {
  text-align: center;
  font-size: 1.5rem;
  margin-bottom: 1rem;
  color: #333;
}

.form-input input {
  padding: 0.7rem 1rem;
  border: 1px solid #ccc;
  border-radius: 0.6rem;
  font-size: 1rem;
}

.form-input input:focus {
  border-color: #6366f1;
  outline: none;
  box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.3);
}

.form-input button {
  padding: 0.8rem;
  background: #6366f1;
  border: none;
  border-radius: 0.6rem;
  font-size: 1rem;
  font-weight: 600;
  color: white;
  cursor: pointer;
  transition: 0.3s;
}

.form-input button:hover {
  background: #4f46e5;
}

.user-table {
  background: #fff;
  padding: 2rem;
  border-radius: 1rem;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
  width: 800px;
}

.user-table h2 {
  text-align: center;
  margin-bottom: 1rem;
  color: #333;
}

.user-table table {
  width: 100%;
  border-collapse: collapse;
  text-align: left;
}

.user-table th,
.user-table td {
  border: 1px solid #ddd;
  padding: 0.8rem;
}

.user-table th {
  background: #6366f1;
  color: white;
}

.user-table tr:nth-child(even) {
  background: #f9f9f9;
}

.user-table tr:hover {
  background: #f1f5f9;
  cursor: pointer;
}

.buttons {
  display: flex;
  gap: 10px;
  justify-content: center;
}

.edit-icon,
.delete-icon {
  width: 22px;
  height: 22px;
  cursor: pointer;
  transition: 0.2s;
}

.edit-icon {
  color: #2563eb;
}

.edit-icon:hover {
  color: #1e40af;
}

.delete-icon {
  color: #dc2626;
}

.delete-icon:hover {
  color: #991b1b;
}
</style>
