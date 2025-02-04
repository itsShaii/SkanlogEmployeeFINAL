<template>
  <el-form @submit.prevent="submitForm" class="employee-form" label-position="top">
    <el-form-item label="Name">
      <el-input v-model="localEmployee.name" required />
    </el-form-item>

    <el-form-item label="Email">
      <el-input v-model="localEmployee.email" required />
    </el-form-item>

    <el-form-item label="Position">
      <el-input v-model="localEmployee.position" required />
    </el-form-item>

    <el-form-item label="Salary">
      <el-input v-model="localEmployee.salary" type="number" required />
    </el-form-item>

    <el-form-item label="SSS Number">
      <el-input v-model="localEmployee.sssNumber" required />
    </el-form-item>

    <el-form-item label="Pag-Ibig Number">
      <el-input v-model="localEmployee.pagIbigNumber" required />
    </el-form-item>

    <el-form-item>
      <el-button type="primary" @click="submitForm">Add Employee</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    newEmployee: Object,
    addEmployee: Function,
  },
  data() {
    return {
      localEmployee: { ...this.newEmployee },
    }
  },
  methods: {
    submitForm() {
      this.addEmployee(this.localEmployee)
      this.localEmployee = { ...this.newEmployee }
    },
  },
})
</script>

<style scoped>
.employee-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 800px;
  margin: 20px auto;
}

.el-form-item {
  display: flex;
  flex-direction: column;
  width: 100%;
  margin-bottom: 15px;
}

.el-form-item__label {
  font-weight: bold;
  color: black;
  margin-bottom: 5px;
}

.el-button {
  width: 100%;
  font-weight: bold;
  background-color: rgb(240, 153, 168);
  border: palevioletred;
}

.el-button:hover {
  background-color: rgb(204, 41, 111);
}
</style>
