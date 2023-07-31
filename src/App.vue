<template>
  <!-- Main container for the application -->
  <div class="container-fluid">
    <div class="row p-0">
      <!-- Sidebar column (visible on larger screens, hidden on smaller screens) -->
      <div class="col-1 d-none d-sm-block">
        <SideBar />
      </div>
      <div class="col-11">
        <!-- Render the EmployeeIndex component, which is a page related to employee management -->
        <EmployeeIndex></EmployeeIndex>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import EmployeeIndex from './components/employees/index.vue'
import SideBar from './components/sidebar.vue'
</script>