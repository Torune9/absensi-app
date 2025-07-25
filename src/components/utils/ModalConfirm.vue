<template>
    <div class="modal fade show d-block" tabindex="-1" role="dialog" style="background-color: rgba(0, 0, 0, 0.5);">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">{{ title }}</h5>
                    <button type="button" class="btn-close" aria-label="Close" @click="$emit('cancel')"></button>
                </div>
                <div class="modal-body">
                    <p>{{ message }}</p>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" @click="$emit('close')">Batal</button>
                    <button type="button" class="btn btn-danger" @click="$emit('confirm',true)">Ya, Lanjutkan</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
defineProps({
    title: {
        type: String,
        default: 'Konfirmasi'
    },
    message: {
        type: String,
        default: 'Apakah Anda yakin?'
    }
})
</script>

<style scoped>
.modal {
    display: block;
}
</style>
