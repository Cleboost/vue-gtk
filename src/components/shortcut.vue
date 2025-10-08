<template>
    <div class="shortcut-container" :class="sizeClass">
        <span class="shortcut-text">{{ text }}</span>
    </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
    text: string;
    size?: 'small' | 'medium' | 'large';
}

const props = withDefaults(defineProps<Props>(), {
    text: 'Ctrl',
    size: 'medium'
});

const sizeClass = computed(() => `shortcut-${props.size}`);
</script>

<style scoped>
.shortcut-container {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    border-radius: 4px;
    font-family: 'Cantarell', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    font-weight: 500;
    user-select: none;
    transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
    position: relative;
    box-shadow: 
        0 1px 0 rgba(255, 255, 255, 0.1),
        0 2px 4px rgba(0, 0, 0, 0.3);
}

.shortcut-text {
    color: inherit;
    font-size: inherit;
    line-height: 1;
    font-weight: 700;
}

.shortcut-small {
    padding: 3px 6px;
    font-size: 11px;
    min-height: 18px;
}

.shortcut-medium {
    padding: 4px 8px;
    font-size: 12px;
    min-height: 20px;
}

.shortcut-large {
    padding: 5px 10px;
    font-size: 14px;
    min-height: 22px;
}

.shortcut-container {
    background-color: #33353B;
    color: #ffffff;
    border: 1px solid #55575C;
}

.shortcut-container::after {
    content: '';
    position: absolute;
    bottom: -2px;
    left: 2px;
    right: 2px;
    height: 2px;
    background-color: #1a1b1f;
    border-radius: 0 0 2px 2px;
    box-shadow: 0 1px 2px rgba(0, 0, 0, 0.4);
}

.shortcut-container:hover {
    transform: translateY(-1px);
    box-shadow: 
        0 1px 0 rgba(255, 255, 255, 0.1),
        0 3px 6px rgba(0, 0, 0, 0.4);
    background-color: #393B3F;
    border-color: #666666;
}

.shortcut-container:hover::after {
    bottom: -3px;
    box-shadow: 0 2px 3px rgba(0, 0, 0, 0.5);
}
</style>
