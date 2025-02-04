<template>
  <div>
    <el-input
      v-model="searchQuery"
      placeholder="Search an employee"
      clearable
      style="width: 1200px; margin-right: 10px; margin-bottom: 10px; border: 2px solid pink"
    />

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
      >{{ success }}
    </el-alert>

    <el-alert
      v-if="warning"
      title="Warning Alert"
      type="warning"
      style="width: 1500px; margin-left: -170px; margin-bottom: 20px"
      show-icon
      >{{ warning }}</el-alert
    >

    <el-table :data="employees" style="width: 100%" border stripe>
      <el-table-column prop="name" label="Name" />
      <el-table-column prop="email" label="Email" />
      <el-table-column prop="position" label="Position" />
      <el-table-column prop="salary" label="Salary" />
      <el-table-column prop="sssNumber" label="SSS Number" />
      <el-table-column prop="pagIbigNumber" label="Pag-Ibig Number" />
      <el-table-column label="Actions">
        <template #default="{ row }">
          <el-button type="primary" @click="openEditDialog(row)">Edit</el-button>
          <el-button type="danger" @click="deleteEmployee(row.id)">Delete</el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- Edit Employee Dialog -->
    <el-dialog v-model="editDialogVisible" title="Edit Employee Details" width="1000px">
      <el-form :model="editEmployee">
        <el-form-item label="Name">
          <el-input v-model="editEmployee.name" />
        </el-form-item>
        <el-form-item label="Email">
          <el-input v-model="editEmployee.email" />
        </el-form-item>
        <el-form-item label="Position">
          <el-input v-model="editEmployee.position" />
        </el-form-item>
        <el-form-item label="Salary">
          <el-input v-model="editEmployee.salary" type="number" />
        </el-form-item>
        <el-form-item label="SSS Number">
          <el-input v-model="editEmployee.sssNumber" />
        </el-form-item>
        <el-form-item label="Pag-Ibig Number">
          <el-input v-model="editEmployee.pagIbigNumber" />
        </el-form-item>
      </el-form>
      <template #footer>
        <el-button @click="editDialogVisible = false">Cancel</el-button>
        <el-button type="primary" @click="updateEmployee">Save</el-button>
      </template>
    </el-dialog>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted, watch } from 'vue'
import axios from 'axios'
import { ElMessageBox } from 'element-plus'

export default defineComponent({
  setup() {
    const employees = ref([])
    const searchQuery = ref('')
    const editDialogVisible = ref(false)
    const editEmployee = ref({})
    const success = ref('')
    const error = ref('')
    const warning = ref('')
    let searchTimeout = null

    // Function to fetch employees based on search query
    const fetchEmployees = async () => {
      try {
        const response = await axios.get(
          `http://localhost:5285/api/employee?search=${searchQuery.value}`,
        )
        employees.value = response.data
      } catch (error) {
        console.error('Error fetching employees:', error)
        error.value = 'Error fetching employees. Please try again later.'
      }
    }

    // Watch for changes in searchQuery and fetch employees with a delay
    watch(searchQuery, () => {
      if (searchTimeout) clearTimeout(searchTimeout)
      searchTimeout = setTimeout(() => {
        fetchEmployees()
      }, 400)
    })

    // Fetch employees on mount
    onMounted(fetchEmployees)

    // Function to delete an employee with confirmation
    const deleteEmployee = async (id) => {
      try {
        await ElMessageBox.confirm('Are you sure you want to delete this employee?', 'Warning', {
          confirmButtonText: 'Yes',
          cancelButtonText: 'No',
          type: 'warning',
        })
        await axios.delete(`http://localhost:5285/api/employee/${id}`)
        fetchEmployees()
        success.value = 'Employee deleted successfully.'
        error.value = ''
      } catch (err) {
        console.error('Error deleting employee:', err)
        if (err !== 'cancel') {
          error.value = 'Failed to delete employee. Please try again later.'
        }
        success.value = ''
        warning.value = 'Deletion cancelled.'
      } finally {
        clearMessages()
      }
    }

    // Open Edit Dialog
    const openEditDialog = (employee) => {
      editEmployee.value = { ...employee }
      editDialogVisible.value = true
    }

    // Update Employee Function
    const updateEmployee = async () => {
      try {
        await axios.put(
          `http://localhost:5285/api/employee/${editEmployee.value.id}`,
          editEmployee.value,
        )
        editDialogVisible.value = false
        fetchEmployees()
        success.value = 'Employee updated successfully.'
        error.value = ''
        warning.value = ''
      } catch (err) {
        console.error('Error updating employee:', err)
        error.value = 'Failed to update employee. Please try again later.'
        success.value = ''
        warning.value = ''
      } finally {
        clearMessages()
      }
    }

    // Clear messages after a timeout
    const clearMessages = () => {
      setTimeout(() => {
        success.value = ''
        error.value = ''
        warning.value = ''
      }, 1000)
    }

    return {
      searchQuery,
      employees,
      fetchEmployees,
      deleteEmployee,
      editDialogVisible,
      editEmployee,
      openEditDialog,
      updateEmployee,
      success,
      error,
      warning,
    }
  },
})
</script>

<style scoped>
.el-button {
  width: auto;
  font-weight: bold;
  margin-right: 5px;
  background-color: rgb(240, 153, 168);
  border: palevioletred;
}

.el-button:hover {
  background-color: rgb(204, 41, 111);
}
</style>
