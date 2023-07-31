<template>
    <!-- Template for the main Employee Master component -->
    <div class="p-5">
        <div class="heading d-flex justify-content-between mt-2 mb-3">
            <div>
                <h3>Employee Master</h3>
            </div>
            <div>
                <!-- Theme switch button with Toggle component -->
                <h5 class="switch-btn">Switch Theme
                    <Toggle v-model="isDarkMode" @click="switchTheme()" />
                </h5>
            </div>
        </div>
        <div class="row">
            <div class="col-sm-12 col-md-6">
                <!-- Child component to create new employees -->
                <CreateVue v-on:create="createEmployee" :employeeData="employeeData"></CreateVue>
            </div>
            <div class="col-sm-12 col-md-6">
                 <!-- Child component to list and edit employees -->
                <ListVue :employee="employee" v-on:edit="editEmployeeData"></ListVue>
            </div>
        </div>
    </div>
</template>
<script setup lang="ts">
import { ref } from 'vue'
import ListVue from './list.vue';
import CreateVue from './create.vue';
import Toggle from '@vueform/toggle'

// Define reactive variables
const isDarkMode = ref(false);
const darkMode = ref(false);
const employee = ref({});
const employeeData = ref({});

// Function to switch the theme (dark/light mode)
const switchTheme = () => {
    if (isDarkMode.value === true) {
         // Add 'dark-mode' class to the body to enable dark mode
        document.querySelector('body').classList.add('dark-mode');
    } else {
        // Remove 'dark-mode' class from the body to disable dark mode
        document.querySelector('body').classList.remove('dark-mode');
    }
}

// Function to handle new employee creation
function createEmployee(formData: object) {
    employee.value = formData;
}

// Function to handle employee data when editing
function editEmployeeData(formData: Object) {
    employeeData.value = formData;
    console.log('employeeData', employeeData);
}
</script>

<style src="@vueform/toggle/themes/default.css"></style>