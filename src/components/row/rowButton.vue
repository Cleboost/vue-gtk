<template>
    <button 
        class="row-button" 
        :class="{ 'last-item': isLast }"
        @click="$emit('click', $event)"
    >
        <div class="row-content">
            <span class="row-text">{{ text }}</span>
            <span v-if="subtitle" class="row-subtitle">{{ subtitle }}</span>
        </div>
        <Icon icon="tabler:arrow-right"/>
    </button>
</template>

<script setup lang="ts">
import { Icon } from '@iconify/vue';
interface Props {
    text: string;
    subtitle?: string;
    isLast?: boolean;
}

defineProps<Props>();

const emit = defineEmits<{
    click: [event: MouseEvent];
}>();
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cantarell:ital,wght@0,400;0,700;1,400;1,700&display=swap');

.row-button {
    width: 100%;
    background-color: #33353B;
    border: none;
    padding: 16px 20px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    cursor: pointer;
    transition: background-color 0.2s ease;
    font-family: 'Cantarell', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    box-sizing: border-box;
    overflow: hidden;
}

.row-button:not(.last-item) {
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.row-button.last-item {
    border-bottom: none;
}

.row-button:hover {
    background-color: #393B3F;
}

.row-button:active {
    background-color: rgba(255, 255, 255, 0.1);
}

.row-content {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 2px;
    flex: 1;
    min-width: 0;
}

.row-text {
    color: #ffffff;
    font-size: 16px;
    font-weight: 400;
    text-align: left;
}

.row-subtitle {
    color: #9ca3af;
    font-size: 14px;
    font-weight: 400;
    text-align: left;
}

.row-button :deep(.iconify) {
    color: #ffffff;
    font-size: 18px;
    flex-shrink: 0;
    margin-left: 12px;
}
</style>
