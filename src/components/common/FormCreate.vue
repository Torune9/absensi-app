<template>
    <div class="card">
        <Modal :mode="modalMode" v-if="isSending" />
        <Alert :mode="alertMode" :message="alertMessage" v-if="alertMode" />
        <div class="card-header"><strong>Form</strong> Absensi</div>
        <div class="card-body">
            <form @submit.prevent="handleSubmit" class="form-horizontal">
                <TextInput label="Nama" id="nama" placeholder="Nama lengkap" v-model:value="form.nama"
                    :help="v$.nama.$errors" />

                <TextInput label="Alamat" id="alamat" placeholder="Jl. Pelajar Pejuang 45 No.10, Bandung"
                    v-model:value="form.alamat" :help="v$.alamat.$errors" />

                <RadioGroup label="Jenis Kelamin" id="jeniskelamin" :options="[
                    { label: 'L', value: 'L' },
                    { label: 'P', value: 'P' }
                ]" v-model:gender="form.jenisKelamin" />

                <TextInput label="Tanggal Absen" id="tanggal" type="date" v-model:value="form.tanggal"
                    :help="v$.tanggal.$errors" />

                <TextInput label="Jam Masuk" id="jamMasuk" type="time" v-model:value="form.jamMasuk"
                    :help="v$.jamMasuk.$errors" />
                <TextInput label="Jam Keluar" id="jamKeluar" type="time" v-model:value="form.jamKeluar"
                    :help="v$.jamKeluar.$errors" />
            </form>
        </div>
        <div class="card-footer">
            <button class="btn btn-sm btn-primary" @click="handleSubmit">
                <i class="fa fa-dot-circle-o"></i> Submit
            </button>
        </div>
    </div>
</template>

<script setup>
import { reactive, ref } from 'vue'

import TextInput from '../input/TextInput.vue'
import RadioGroup from '../input/RadioGroup.vue'
import useVuelidate from '@vuelidate/core'
import { required } from '@vuelidate/validators'
import Alert from '../utils/Alert.vue'
import Modal from '../utils/Modal.vue'

const form = reactive({
    nama: '',
    jenisKelamin: '',
    alamat: '',
    tanggal: '',
    jamMasuk: '',
    jamKeluar: ''
})
const rules = {
    nama: { required },
    jenisKelamin: { required },
    alamat: { required },
    tanggal: { required },
    jamMasuk: { required },
    jamKeluar: { required }
}

const v$ = useVuelidate(rules, form)

const resetForm = () => {
    form.nama = ''
    form.jenisKelamin = ''
    form.alamat = ''
    form.tanggal = ''
    form.jamMasuk = ''
    form.jamKeluar = ''
}

const isSending = ref(false)

const alertMode = ref('')
const alertMessage = ref('')
const modalMode = ref('info')

const emit = defineEmits(['handleSuccess'])
const handleSubmit = async () => {
    v$.value.$touch()
    if (v$.value.$error) {
        return
    }
    isSending.value = !isSending.value
    try {
        await fetch('http://localhost:3000/absensi', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ ...form })
        })
        alertMode.value = 'success'
        alertMessage.value = 'Berhasil absen'

        emit('handleSuccess',true)
    } catch (error) {
        alertMode.value = 'error'
        alertMessage.value = 'Gagal absen'
    } finally {
        isSending.value = !isSending.value
        resetForm()
        v$.value.$reset()
    }
}
</script>
