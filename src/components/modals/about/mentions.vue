<template>
    <div class="about-mentions">
        <div class="mentions-header">
            <button class="back-button" @click="$emit('close')">
                <Icon icon="tabler:arrow-left" />
            </button>
            <h2 class="mentions-title">Mentions légales</h2>
        </div>
        
        <div class="mentions-content">
            <p class="mentions-text">{{ mentionLegales }}</p>
        </div>
    </div>
</template>

<script setup lang="ts">
import { Icon } from '@iconify/vue';

interface Props {
    mentionLegales: string;
}

defineProps<Props>();

defineEmits<{
    close: [];
}>();
</script>

<style scoped>
@import '../../../css/row.css';

.about-mentions {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #26282D;
    border-radius: 12px;
    padding: 20px;
    display: flex;
    flex-direction: column;
    gap: 16px;
    z-index: 10;
    min-height: 500px;
    box-sizing: border-box;
    overflow-y: auto;
    max-height: 70vh;
    transition: all 0.6s cubic-bezier(0.25, 0.46, 0.45, 0.94);
    transform-origin: center center;
}

.mentions-header {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 8px;
}

.back-button {
    background: none;
    border: none;
    color: #ffffff;
    font-size: 20px;
    cursor: pointer;
    padding: 8px;
    border-radius: 50%;
    transition: background-color 0.2s ease;
    display: flex;
    align-items: center;
    justify-content: center;
}

.back-button:hover {
    background-color: rgba(255, 255, 255, 0.1);
}

.mentions-title {
    color: #ffffff;
    font-size: 20px;
    font-weight: 600;
    margin: 0;
}

.mentions-content {
    display: flex;
    flex-direction: column;
    gap: 16px;
}

.mentions-text {
    color: #ffffff;
    font-size: 14px;
    line-height: 1.6;
    margin: 0;
}
</style>
