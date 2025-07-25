<template>
    <header class="app-header bg-light border-0 navbar">
        <button class="navbar-toggler sidebar-toggler d-lg-none mr-auto ml-3" type="button"
            @click="$emit('toggle-sidebar')">
            <span class="navbar-toggler-icon"></span>
        </button>

        <a class="navbar-brand" href="#"><b>Back</b><span>strap</span></a>

        <button class="navbar-toggler sidebar-toggler d-md-down-none" type="button" @click="$emit('toggle-sidebar')">
            <span class="navbar-toggler-icon"></span>
        </button>

        <ul class="nav navbar-nav d-md-down-none">
            <li class="nav-item px-3"><a class="nav-link" href="#">Dashboard</a></li>
            <li class="nav-item px-3"><a class="nav-link" href="#">Users</a></li>
            <li class="nav-item px-3"><a class="nav-link" href="#">Settings</a></li>
        </ul>

        <ul class="nav navbar-nav ml-auto">
            <li class="nav-item d-md-down-none"><a class="nav-link" href="#"><i class="icon-bell"></i><span
                        class="badge badge-pill badge-danger">5</span></a></li>
            <li class="nav-item dropdown">
                <a class="nav-link" href="#"><img class="img-avatar" :src="img6" alt="admin" /></a>
            </li>
        </ul>
    </header>
</template>

<script setup>
import img6 from '@/assets/img/avatars/6.jpg'
</script>
