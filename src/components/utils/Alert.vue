<template>
    <div class="alert" role="alert" :class="`alert-${mode}`">
        {{ message }}
    </div>
</template>
<script setup>
const props = defineProps({
    mode: String,
    message : String
})
</script>
