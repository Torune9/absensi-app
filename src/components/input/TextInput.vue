<template>
    <div class="form-group row">
        <label :for="id" class="col-md-3 col-form-label">{{ label }}</label>
        <div class="col-md-9">
            <input :id="id" :type="type" class="form-control" :placeholder="placeholder" v-model="modelValue"/>
            <span v-if="help" v-for="h in help" class="help-block text-danger">{{ h.$message }}</span>
        </div>
    </div>
</template>

<script setup>
defineProps({
    label: String,
    id: String,
    placeholder: String,
    help: Array,
    type: {
        type: String,
        default: 'text'
    },
})

const modelValue = defineModel('value')
</script>
