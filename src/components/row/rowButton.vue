<template>
    <button 
        class="row-button row-base" 
        :class="{ 'last-item': isLast }"
        @click="$emit('click', $event)"
    >
        <div class="row-content">
            <span class="row-text">{{ text }}</span>
            <span v-if="subtitle" class="row-subtitle">{{ subtitle }}</span>
        </div>
        <Icon icon="tabler:arrow-right" class="row-icon"/>
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
@import '../../css/row.css';
</style>
