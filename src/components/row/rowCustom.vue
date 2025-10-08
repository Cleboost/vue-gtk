<template>
    <div 
        class="row-custom row-base" 
        :class="{ 'last-item': isLast }"
    >
        <slot></slot>
    </div>
</template>

<script setup lang="ts">
interface Props {
    isLast?: boolean;
}

defineProps<Props>();
</script>

<style scoped>
@import '../../css/row.css';
</style>

