<script setup lang="ts">
import UserCard from "@/components/UserCard.vue";
import { userStore } from "@/stores/UserStore";
import { defineModel, onMounted, onUnmounted } from 'vue';

const model = defineModel<string>();
const store = userStore();

const search = () => {
    if (model.value === undefined) {
        return;
    }

    store.trySearch(model.value);
}

const onScroll = () => {
    const position = window.scrollY + window.innerHeight;
    const docHeight = document.documentElement.scrollHeight;

    if (position + 50 >= docHeight) {
        store.loadMore();
    }
}

onMounted(() => {
    store.fetch();
    window.addEventListener('scroll', onScroll);
});

onUnmounted(() => {
    window.removeEventListener('scroll', onScroll);
})
</script>

<template>
    <div id="search-container">
        <el-input
        v-model="model" style="width: 90vw; margin: 5vw;"
        placeholder="Search users here" @input="search" />
        
        <UserCard v-for="user in store.users" :user="user"/>
    </div>
</template>

<style>
#search-container {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
}
</style>