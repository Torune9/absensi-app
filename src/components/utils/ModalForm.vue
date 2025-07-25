<template>
    <div class="modal fade show d-block" tabindex="-1" role="dialog" style="background-color: rgba(0,0,0,0.5);">
        <div class="modal-dialog modal-lg" role="document">
            <div class="modal-content">
                <!-- Header -->
                <div class="modal-header">
                    <h5 class="modal-title">Edit Data Absensi</h5>
                    <button type="button" class="close btn" @click="$emit('close')">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>

                <!-- Body -->
                <div class="modal-body">
                    <form @submit.prevent="handleSubmit" class="form-horizontal">
                        <TextInput label="Nama" id="nama" placeholder="Nama lengkap" v-model:value="form.nama" />
                        <TextInput label="Alamat" id="alamat" placeholder="Jl. Pelajar Pejuang 45 No.10, Bandung"
                            v-model:value="form.alamat" />
                        <RadioGroup label="Jenis Kelamin" id="jeniskelamin" :options="[
                            { label: 'L', value: 'L' },
                            { label: 'P', value: 'P' }
                        ]" v-model:gender="form.jenisKelamin" />
                    </form>
                </div>

                <!-- Footer -->
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" @click="$emit('close')">Tutup</button>
                    <button type="submit" class="btn btn-primary" @click="handleSubmit">Simpan</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { reactive } from 'vue'
import TextInput from '../input/TextInput.vue'
import RadioGroup from '../input/RadioGroup.vue'

// Props
const props = defineProps({
    data: {
        type: Object,
    }
})

// Reactive Form
const form = reactive({
    nama: props.data?.nama || '',
    alamat: props.data?.alamat || '',
    jenisKelamin: props.data?.jenisKelamin || ''
})

// Emit close or update
const emit = defineEmits(['close', 'updated'])

// Handle Submit
const handleSubmit = async () => {
    try {
        const response = await fetch(`http://localhost:3000/absensi/${props.data.id}`, {
            method: 'PATCH',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(form)
        })
        if (response.ok) {            
            const result = await response.json()
            emit('updated', result)
            emit('close')
        } else {
            console.error('Update failed')
        }
    } catch (error) {
        console.error(error)
    }
}
</script>
