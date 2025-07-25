<template>
    <div class="sidebar sidebar-pills bg-transparent">
        <nav class="sidebar-nav">
            <ul class="nav">
                <LinkNav name="Dashboard" icon="lni-dashboard" @click="selectMenu"/>
                <li class="nav-title">Absence</li>
                <LinkNav name="Create" icon="lni-target" @click="selectMenu"/>
            </ul>
        </nav>
    </div>
</template>

<script setup>
import LinkNav from '../utils/LinkNav.vue';
const emits = defineEmits(['pickMenu'])
const selectMenu = (name) => {
  emits('pickMenu', name)
}
</script>a
