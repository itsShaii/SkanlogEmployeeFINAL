<template>
  <div>
    <h1 class="title">
      <el-icon class="icon"><Avatar /></el-icon>
      Skanlog Employee Management System
    </h1>

    <el-alert
      v-if="error"
      title="Error Alert"
      type="error"
      style="width: 1500px; margin-left: -170px; margin-bottom: 20px"
      show-icon
      >{{ error }}
    </el-alert>
    <el-alert
      v-if="success"
      title="Success Alert"
      type="success"
      style="width: 1500px; margin-left: -170px; margin-bottom: 20px"
      show-icon
      >{{ success }}</el-alert
    >
    <el-alert
      v-if="warning"
      title="Warning Alert"
      type="warning"
      style="width: 1500px; margin-left: -170px; margin-bottom: 20px"
      show-icon
      >{{ warning }}</el-alert
    >

    <div class="content">
      <div class="form-container">
        <EmployeeForm :newEmployee="newEmployee" :addEmployee="addEmployee" />
      </div>

      <div class="table-container">
        <EmployeeTable :employees="employees" :deleteEmployee="deleteEmployee" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import { defineComponent } from 'vue'
import EmployeeForm from '../components/EmployeeForm.vue'
import EmployeeTable from '../components/EmployeeTable.vue'
import { Avatar } from '@element-plus/icons-vue'

export default defineComponent({
  components: { EmployeeForm, EmployeeTable, Avatar },
  data() {
    return {
      employees: [],
      newEmployee: {
        name: '',
        email: '',
        position: '',
        salary: '',
        sssNumber: '',
        pagIbigNumber: '',
      },
      error: '',
      success: '',
      warning: '',
    }
  },
  methods: {
    // Fetch Employees
    async fetchEmployees() {
      try {
        const response = await axios.get('http://localhost:5285/api/Employee')
        this.employees = response.data
      } catch {
        this.error = 'Failed to fetch employees.'
        this.success = ''
        this.warning = ''
      }
    },

    // Add Employee (Receives employee object from child)
    async addEmployee(employeeData) {
      try {
        const response = await axios.post('http://localhost:5285/api/Employee', {
          name: employeeData.name,
          email: employeeData.email,
          position: employeeData.position,
          salary: parseFloat(employeeData.salary),
          sssNumber: employeeData.sssNumber,
          pagIbigNumber: employeeData.pagIbigNumber,
        })
        this.employees.push(response.data)
        this.success = 'Employee added successfully.'
        this.error = ''
        this.warning = ''
      } catch (err) {
        console.error(err.response?.data || err.message)
        this.error = 'Failed to add employee. Ensure all fields are correctly formatted.'
        this.success = ''
        this.warning = ''
      }
    },
  },
  mounted() {
    this.fetchEmployees()
  },
})
</script>

<style scoped>
.title {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 50px;
  font-weight: bold;
  color: black;
  margin-bottom: 20px;
  margin-top: 30px;
}

.icon {
  font-size: 60px;
  color: rgb(204, 41, 111);
  margin-left: -170px;
}

.content {
  display: flex;
  width: 120%;
  gap: 20px;
  margin-left: -170px;
}

.form-container {
  width: 150%;
  padding: 20px;
  border-radius: 8px;
  background-color: white;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  border: 2px solid pink;
}

.table-container {
  flex: 1;
  padding: 20px;
  border-radius: 8px;
  background-color: white;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  border: 2px solid pink;
}

@media (max-width: 768px) {
  .content {
    flex-direction: column;
  }
  .form-container,
  .table-container {
    width: 100%;
  }
}
</style>
