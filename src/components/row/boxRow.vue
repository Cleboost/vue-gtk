<template>
    <div class="box-row">
        <slot />
    </div>
</template>

<script setup lang="ts">
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cantarell:ital,wght@0,400;0,700;1,400;1,700&display=swap');

.box-row {
    background-color: #2d2d2d;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 0 5px 0 rgba(0, 0, 0, 0.2);
}
</style>
