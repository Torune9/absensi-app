<template>
    <ul class="pagination">
        <!-- Prev button -->
        <li class="page-item" :class="{ disabled: currentPage === 1 }">
            <a class="page-link" href="#" @click.prevent="changePage(currentPage - 1)">Prev</a>
        </li>

        <li v-for="page in visiblePages" :key="page" class="page-item" :class="{ active: currentPage === page }">
            <a class="page-link" href="#" @click.prevent="changePage(page)">{{ page }}</a>
        </li>

        <li class="page-item" :class="{ disabled: currentPage === totalPages }">
            <a class="page-link" href="#" @click.prevent="changePage(currentPage + 1)">Next</a>
        </li>
    </ul>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
    totalPages: Number,
    currentPage: Number
})

const emit = defineEmits(['page-change'])

const changePage = (page) => {
    if (page >= 1 && page <= props.totalPages) {
        emit('page-change', page)
    }
}

const visiblePages = computed(() => {
    const maxVisible = 5
    let start = props.currentPage - Math.floor(maxVisible / 2)
    let end = props.currentPage + Math.floor(maxVisible / 2)

    if (start < 1) {
        start = 1
        end = Math.min(maxVisible, props.totalPages)
    }

    if (end > props.totalPages) {
        end = props.totalPages
        start = Math.max(1, end - maxVisible + 1)
    }

    const pages = []
    for (let i = start; i <= end; i++) {
        pages.push(i)
    }

    return pages
})
</script>
