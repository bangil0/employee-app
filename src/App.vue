<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>

    <EmployeeForm @add:employee="addEmployee" />
    <EmployeeTable
      :employees="employees"
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"
    />
  </div>
</template>

<script>
import EmployeeTable from './components/EmployeeTable'
import EmployeeForm from './components/EmployeeForm'
import axios from 'axios'

export default {
  name: 'app',
  components: {
    EmployeeTable,
    EmployeeForm
  },
  data() {
    return {
      employees: []
    }
  },
  methods: {
    async getEmployees() {
      try {
        const res = await axios.get(
          'https://jsonplaceholder.typicode.com/users?_limit=3'
        )
        this.employees = res.data
      } catch (err) {
        console.error(err)
      }
    },
    async addEmployee(employee) {
      const res = await axios.post(
        'https://jsonplaceholder.typicode.com/users',
        employee
      )
      this.employees = [...this.employees, res.data]
    },
    async deleteEmployee(id) {
      await axios.delete(`https://jsonplaceholder.typicode.com/users/${id}`)
      this.employees = this.employees.filter(employee => employee.id !== id)
    },
    async editEmployee(id, updatedEmployee) {
      try {
        const res = await axios.put(
          `https://jsonplaceholder.typicode.com/users/${id}`,
          updatedEmployee
        )
        this.employees = this.employees.map(employee =>
          employee.id === id ? res.data : employee
        )
      } catch (err) {
        console.error(err)
      }
    }
  },
  mounted() {
    this.getEmployees()
  }
}
</script>

<style>
button {
  background: #009435;
  border: 1px solid #009435;
}

.small-container {
  max-width: 680px;
}
</style>
