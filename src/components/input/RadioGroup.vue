<template>
    <div class="form-group row">
        <label class="col-md-3 col-form-label">{{ label }}</label>
        <div class="col-md-9 col-form-label">
            <div class="form-check form-check-inline mr-1" v-for="option in options" :key="option.value">
                <input class="form-check-input" type="radio" :id="`${id}-${option.value}`" :name="id"
                    :value="option.value" v-model="modelValue" />
                <label class="form-check-label" :for="`${id}-${option.value}`">{{ option.label }}</label>
            </div>
        </div>
    </div>
</template>

<script setup>
defineProps({
    label: String,
    id: String,
    options: Array,
})

const modelValue = defineModel('gender')

</script>
