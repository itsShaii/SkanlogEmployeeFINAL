<!-- <template>
  <div>
    <h1>Skanlog Employee Management System</h1>

    <!-- Display Error Messages -->
    <el-alert v-if="error" type="error" :closable="false" show-icon>{{ error }}</el-alert>

    <!-- Add Employee Form -->
    <el-form @submit.prevent="addEmployee" label-width="120px" class="employee-form">
      <el-form-item label="Name">
        <el-input v-model="newEmployee.name" placeholder="Enter name" required />
      </el-form-item>

      <el-form-item label="Email">
        <el-input v-model="newEmployee.email" placeholder="Enter email" required />
      </el-form-item>

      <el-form-item label="Position">
        <el-input v-model="newEmployee.position" placeholder="Enter position" required />
      </el-form-item>

      <el-form-item label="Salary">
        <el-input v-model="newEmployee.salary" type="number" placeholder="Enter salary" required />
      </el-form-item>

      <el-form-item label="SSS Number">
        <el-input v-model="newEmployee.sssNumber" placeholder="Enter SSS Number" required />
      </el-form-item>

      <el-form-item label="Pag-IBIG Number">
        <el-input
          v-model="newEmployee.pagIbigNumber"
          placeholder="Enter Pag-IBIG Number"
          required
        />
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="addEmployee">Add Employee</el-button>
      </el-form-item>
    </el-form>

    <!-- Employee Table -->
    <el-table :data="employees" style="width: 80%; margin: 20px auto" border stripe>
      <el-table-column prop="name" label="Name" />
      <el-table-column prop="email" label="Email" />
      <el-table-column prop="position" label="Position" />
      <el-table-column prop="salary" label="Salary" />
      <el-table-column prop="sssNumber" label="SSS Number" />
      <el-table-column prop="pagIbigNumber" label="Pag-IBIG Number" />
      <el-table-column label="Action">
        <template #default="{ row }">
          <el-button type="danger" @click="deleteEmployee(row.id)">Delete</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import axios from 'axios'
import { defineComponent } from 'vue'

export default defineComponent({
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
    }
  },
  methods: {
    // Fetch Employees
    async fetchEmployees() {
      try {
        const response = await axios.get('http://localhost:5285/api/Employee')
        this.employees = response.data
      } catch (err) {
        this.error = 'Failed to fetch employees.'
      }
    },

    // Add Employee
    async addEmployee() {
      try {
        const response = await axios.post('http://localhost:5285/api/Employee', {
          name: this.newEmployee.name,
          email: this.newEmployee.email,
          position: this.newEmployee.position,
          salary: parseFloat(this.newEmployee.salary),
          sssNumber: this.newEmployee.sssNumber,
          pagIbigNumber: this.newEmployee.pagIbigNumber,
        })
        this.employees.push(response.data)
        this.newEmployee = {
          name: '',
          email: '',
          position: '',
          salary: '',
          sssNumber: '',
          pagIbigNumber: '',
        }
      } catch (err) {
        console.error(err.response?.data || err.message)
        this.error = 'Failed to add employee. Ensure all fields are correctly formatted.'
      }
    },

    // Delete Employee
    async deleteEmployee(id) {
      try {
        await axios.delete(`http://localhost:5285/api/Employee/${id}`)
        this.employees = this.employees.filter((emp) => emp.id !== id)
      } catch (err) {
        console.error(err.response?.data || err.message)
        this.error = 'Failed to delete employee.'
      }
    },
  },
  mounted() {
    this.fetchEmployees()
  },
})
</script>

<!-- <style scoped>
h1 {
  text-align: center;
}

.employee-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 20px;
}

.el-alert {
  width: 50%;
  margin: 0 auto 10px;
}
</style> --> -->
