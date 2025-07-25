<template>
  <div class="col-lg-14">
    <Alert :mode="alertMode" :message="alertMessage" />
    <ModalConfirm v-if="showModalConfirm" @confirm="confirmDelete" @close="closeModal" />
    <ModalForm v-if="showModal" :data="dataUpdate" @close="closeModal" @updated="handleInfoUpdate" />
    <div class="card">
      <div class="card-body">
        <!-- Dropdown -->
        <div class="mb-3 d-flex align-items-center justify-content-between">
          <h5>Absensi</h5>
          <div class="dropdown" @click="toggleDropdown" style="position: relative;">
            <button class="btn btn-secondary dropdown-toggle" type="button">
              Sort by: {{ selectedSortLabel }}
            </button>
            <div class="dropdown-menu show" v-if="dropdownOpen" style="position: absolute; right: 0; z-index: 1000;">
              <a class="dropdown-item" href="#" @click.prevent="sortBy('nama')">Nama</a>
              <a class="dropdown-item" href="#" @click.prevent="sortBy('tanggal')">Tanggal</a>
              <a class="dropdown-item" href="#" @click.prevent="sortBy('jamMasuk')">Jam Masuk</a>
            </div>
          </div>
        </div>
        <!-- Table -->
        <table class="table table-responsive-sm table-bordered">
          <thead>
            <tr>
              <th>Nama</th>
              <th>Tanggal Absen</th>
              <th>Jam Masuk</th>
              <th>Jam Keluar</th>
              <th>Jenis Kelamin</th>
              <th>Alamat</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in data" :key="item.id">
              <td>{{ item.nama }}</td>
              <td>{{ item.tanggal }}</td>
              <td>{{ item.jamMasuk }}</td>
              <td>{{ item.jamKeluar }}</td>
              <td>{{ item.jenisKelamin }}</td>
              <td>{{ item.alamat }}</td>
              <td>
                <div class="d-flex">
                  <button class="btn btn-outline-info mr-2" type="button" @click="handleUpdate(item)">Update</button>
                  <button class="btn btn-outline-danger" type="button" @click="showConfirm(item.id)">Delete</button>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
        <Pagination :totalPages="totalPages" :currentPage="currentPage" @page-change="goToPage" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
import Pagination from './Pagination.vue'
import ModalForm from '../utils/ModalForm.vue'
import Alert from '../utils/Alert.vue'
import ModalConfirm from '../utils/ModalConfirm.vue'

const data = ref([])
const currentPage = ref(1)
const itemsPerPage = 6
const totalPages = ref(1)
const deleteId = ref(null)

const dropdownOpen = ref(false)
const selectedSort = ref('')
const selectedSortLabel = ref('Default')


const dataUpdate = ref(null)
const showModal = ref(false)

const showModalConfirm = ref(false)

const alertMode = ref('')
const alertMessage = ref('')

const toggleDropdown = () => {
  dropdownOpen.value = !dropdownOpen.value
}

const sortBy = (field) => {
  selectedSort.value = field
  selectedSortLabel.value = field.charAt(0).toUpperCase() + field.slice(1)
  getData()
}

const getData = async () => {
  try {
    const url = new URL('http://localhost:3000/absensi')
    url.searchParams.set('_page', currentPage.value)
    url.searchParams.set('_per_page', itemsPerPage)

    if (selectedSort.value) {
      url.searchParams.set('_sort', selectedSort.value)
    }

    const response = await fetch(url)
    if (response.ok) {
      const result = await response.json()
      const totalItems = result.items
      data.value = result.data
      totalPages.value = Math.ceil(totalItems / itemsPerPage)
    }
  } catch (error) {
    alertMode.value = 'danger'
    alertMessage.value = 'gagal mendapatkan data'
  }
}

const handleUpdate = (item) => {
  showModal.value = true
  dataUpdate.value = item
}
const closeModal = () => {
  showModal.value = false
  showModalConfirm.value = false
}
const handleInfoUpdate = (data) => {
  if (data) {
    alertMode.value = 'success'
    alertMessage.value = 'Update data berhasil'
  }
  getData()
}

const showConfirm = (id) => {
  showModalConfirm.value = true
  deleteId.value = id
}

const confirmDelete = async (isDelete) => {
  try {
    if (isDelete) {
      const response = await fetch(`http://localhost:3000/absensi/${deleteId.value}`, {
        method: 'DELETE'
      })
      if (response.ok) {
        alertMode.value = 'success'
      }
    }
    getData()
  } catch (error) {
    alertMode.value = 'danger'
    console.error('Error fetching data:', error)
  } finally {
    showModalConfirm.value = false
    alertMessage.value = 'Delete data berhasil'
  }
}

const goToPage = (page) => {
  if (page >= 1 && page <= totalPages.value) {
    currentPage.value = page
  }
}

watch(currentPage, getData, { immediate: true })

</script>
