<template>
    <div class="spinner-container" :class="sizeClass">
        <svg class="spinner-svg" :width="size" :height="size" viewBox="0 0 40 40">
            <circle 
                class="spinner-track" 
                cx="20" 
                cy="20" 
                r="16" 
                :stroke="trackColor"
                :stroke-width="strokeWidth"
                fill="none"
            />
            
            <circle 
                class="spinner-spinner" 
                cx="20" 
                cy="20" 
                r="16" 
                :stroke="color"
                :stroke-width="strokeWidth"
                fill="none"
                stroke-linecap="round"
                :stroke-dasharray="dashArray"
                stroke-dashoffset="0"
            />
        </svg>
    </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
    size?: 'small' | 'medium' | 'large';
    strokeWidth?: number;
    color?: string;
    trackColor?: string;
    speed?: 'slow' | 'normal' | 'fast';
}

const props = withDefaults(defineProps<Props>(), {
    size: 'medium',
    strokeWidth: 8,
    color: '#ffffff',
    trackColor: '#424349',
    speed: 'normal'
});

const sizeClass = computed(() => `spinner-${props.size}`);

const size = computed(() => {
    switch (props.size) {
        case 'small': return 40;
        case 'large': return 120;
        default: return 80;
    }
});

const dashArray = computed(() => {
    const circumference = 2 * Math.PI * 16;
    const dashLength = circumference / 3;
    const gapLength = (circumference * 2) / 3;
    return `${dashLength}, ${gapLength}`;
});

const animationDuration = computed(() => {
    switch (props.speed) {
        case 'slow': return '3s';
        case 'fast': return '0.8s';
        default: return '1.5s';
    }
});
</script>

<style scoped>
.spinner-container {
    position: relative;
    display: inline-block;
}

.spinner-svg {
    animation: rotate v-bind('animationDuration') linear infinite;
}

@keyframes rotate {
    100% {
        transform: rotate(360deg);
    }
}

.spinner-small {
    width: 40px;
    height: 40px;
}

.spinner-medium {
    width: 80px;
    height: 80px;
}

.spinner-large {
    width: 120px;
    height: 120px;
}
</style>
