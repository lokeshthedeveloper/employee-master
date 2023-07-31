<template>
  <!-- Template for the Employee Information Form -->
  <div class="p-5 form-container">
    <div class="heading d-flex justify-content-between mt-2 mb-3">
      <div>
        <h6 class="white-color">Employee Info</h6>
      </div>
      <div><a class="edit-button white-color" @click="form.mode = 'edit'"><font-awesome-icon icon="pencil"
            class="pencil-icon" />EDIT</a></div>
    </div>
    <!-- Employee information form -->
    <form @submit.prevent="submitForm">
      <div class="form-group mb-3">
        <input type="text" :readonly="(form.mode == 'view')" required v-model="form.displayName" class="form-control input-box" id="displayName"
          placeholder="Display Name">
      </div>
      <div class="form-group mb-3">
        <input type="email" :readonly="(form.mode == 'view')" v-model="form.loginId" class="form-control input-box" id="loginId" placeholder="Login ID">
      </div>
      <div class="form-group mb-3">
        <input type="password" :readonly="(form.mode == 'view')" v-model="form.password" class="form-control input-box" id="password"
          placeholder="Password">
      </div>
      <div class="form-group mb-3">
        <div class="d-flex justify-content-between">
          <div class="white-color"><label class="status">Status</label></div>
          <div class="white-color">
            <Toggle :disable="(form.mode == 'view')" v-model="form.status" /> {{  form.status ? 'Active' : 'Inactive' }}
          </div>
        </div>
      </div>
      <div class="form-group mb-3 mt-5" v-if="form.mode == 'edit'">
        <div class="d-flex justify-content-center">
          <button type="submit" class="form-button">Save</button>
        </div>
      </div>
    </form>
  </div>
</template>

<script setup lang="ts">
// Import necessary components and functions
import Toggle from '@vueform/toggle'
import { ref, watch } from 'vue'
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

// Define a reactive form object to store employee data
const form = ref({
  id: null,
  displayName: null,
  loginId: null,
  password: null,
  status: false,
  mode: 'edit'
});

// Define props and emit function
const props = defineProps({
  employeeData: Object
});

// Define emit function to send form data on submission
const employeeFormSubmit = defineEmits(['create']);

// Watch the 'employeeData' prop for changes inside the setup function
watch(() => props.employeeData, (newEmployee, oldEmployee) => {
  // This function will be called whenever the 'employeeData' prop changes
  if (newEmployee) {
    form.value = newEmployee;
  }
}, { deep: true });

// Function to handle form submission
function submitForm() {
  var formData = Object.assign({}, form.value);
  employeeFormSubmit('create', formData);
  form.value = {
    id: null,
    displayName: null,
    loginId: null,
    password: null,
    status: false,
    mode: 'edit'
  }
}
</script>
