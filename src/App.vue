<template>
  <div class="aside-menu-fixed" :class="{ 'sidebar-show': isSidebarVisible }">
    <NavBar @toggle-sidebar="toggleSidebar" />
    <div class="app-body">
      <SideBar v-if="isSidebarVisible" @pick-menu="getMenuName" />
      <main class="main">
        <BreadCrumb />
        <div class="container-fluid">
          <component :is="tabs[currentTab]" @handleSuccess="handleSuccess"/>
        </div>
      </main>
    </div>
  </div>
</template>


<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

import NavBar from '@/components/navigation/NavBar.vue'
import SideBar from '@/components/navigation/SideBar.vue'
import BreadCrumb from '@/components/common/BreadCrumb.vue'
import Table from '@/components/common/Table.vue'
import FormCreate from '@/components/common/FormCreate.vue'

const currentTab = ref('Dashboard')
const isSidebarVisible = ref(true)
const screenWidth = ref(window.innerWidth)

const handleResize = () => {
  screenWidth.value = window.innerWidth
  if (screenWidth.value < 1024) {
    isSidebarVisible.value = false
  } else {
    isSidebarVisible.value = true
  }
}

const tabs = {
  Dashboard: Table,
  Create: FormCreate
}

const handleSuccess = (created)=>{
  if (created) {
    currentTab.value = 'Dashboard'
  }
}

onMounted(() => {
  window.addEventListener('resize', handleResize)
  handleResize()
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', handleResize)
})

const toggleSidebar = () => {
  if (screenWidth.value < 1024) {
    isSidebarVisible.value = !isSidebarVisible.value
  }
}

const getMenuName = (name) => {
  currentTab.value = name
}
</script>
