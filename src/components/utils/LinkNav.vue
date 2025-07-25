<template>
  <li class="nav-item">
    <a class="nav-link" href="#" @click.prevent="handleClick">
      <i :class="`lni ${icon}`"></i> {{ name }}
    </a>
  </li>
</template>

<script setup>
const props = defineProps({
  name: String,
  icon: String
})
const emit = defineEmits(['click'])

const handleClick = () => {
  emit('click', props.name)
}
</script>
