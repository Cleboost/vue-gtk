<template>
    <div class="row-shortcut row-base" :class="{ 'last-item': isLast }">
        <div class="row-content">
            <span class="row-text">{{ title }}</span>
            <span v-if="description" class="row-subtitle">{{ description }}</span>
        </div>
        <div class="row-shortcuts">
            <Shortcut 
                v-for="(key, index) in keys" 
                :key="index"
                :text="key"
                size="small"
            />
        </div>
    </div>
</template>

<script setup lang="ts">
import Shortcut from '../shortcut.vue';
import '../../css/row.css';

interface Props {
    title: string;
    description?: string;
    keys: string[];
    isLast?: boolean;
}

withDefaults(defineProps<Props>(), {
    description: '',
    isLast: false
});
</script>

<style scoped>
.row-shortcuts {
    display: flex;
    align-items: center;
    gap: 6px;
    flex-shrink: 0;
    margin-left: 12px;
}
</style>
