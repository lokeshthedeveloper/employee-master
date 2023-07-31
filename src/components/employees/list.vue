<template>
  <!-- Template for the Employee List View -->
  <div class="p-5 form-container list-view">
    <div class="heading d-flex justify-content-between mt-2 mb-3">
      <div class="col">
        <!-- Display the number of employees in the list -->
        <h6 class="white-color">View Employees ({{ employees.length }})</h6>
      </div>
      <div class="col-4">
        <!-- Search form -->
        <form class="form-inline position-relative search-form">
          <div class="input-group">
            <div class="input-group-prepend position-absolute search-icon">
              <font-awesome-icon icon="search" />
            </div>
            <!-- Input field for search -->
            <input type="text" v-model="searchQuery" class="form-control" placeholder="Search" aria-label="Username"
              aria-describedby="basic-addon1">
          </div>
        </form>
      </div>
    </div>
    <ul class="list-unstyled">
      <!-- Show employees in a list if there are filtered items -->
      <li class="media" v-if="filteredItems.length > 0">
        <!-- Employee details -->
        <div class="row mb-3" v-for="(emp, index) in filteredItems" :key="index">
          <div class="col-sm-3 col-12">
            <div class="image-box"><img class="mr-3 img-fluid" src="../../assets/user.png"></div>
          </div>
          <div class="col-sm-6 col-6">
            <div class="list-text white-color">{{ emp['displayName'] }} <br> {{ emp['loginId'] }} </div>
          </div>
          <div class="col-sm-3 col-6"><a class="list-button" @click="editEmployee(index)">View</a></div>
        </div>
      </li>
      <!-- Show a message if no employees are found -->
      <li v-else>Employees not found.</li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import { watch, ref, computed } from 'vue';

// Define props and emit function
const props = defineProps({
  employee: Object
});
const editEmployeeData = defineEmits(['edit']);

// Define reactive variables
const employees = ref([]);
const searchQuery = ref('');

// Watch the 'employee' prop for changes inside the setup function
watch(() => props.employee, (newEmployee, oldEmployee) => {
  // This function will be called whenever the 'employee' prop changes
  if (newEmployee) {
    createEmployee(newEmployee);
  }
}, { deep: true });

// Function to create or update an employee in the list
function createEmployee(newEmployee: Object) {
  if (newEmployee.id || newEmployee.id === 0) {
    employees.value.splice(newEmployee.id, 1, newEmployee);
  } else {
    employees.value.push(newEmployee);
  }
}

// Function to view an employee's details
function editEmployee(index: number) {
  employees.value[index].id = index;
  employees.value[index].mode = 'view';
  var employeeData = Object.assign({}, employees.value[index]);
  editEmployeeData('edit', employeeData);
}

// Use a computed property to filter the items based on the search query
const filteredItems = computed(() => {
  if (searchQuery.value === '') {
    return employees.value;
  } else {
    return employees.value.filter(item => {
      // Check if any field of the item matches the search query
      return Object.values(item).some(value =>
      value ? value.toString().toLowerCase().indexOf(searchQuery.value.toLowerCase()) !== -1 : false
      );
    });
  }
});
</script>
