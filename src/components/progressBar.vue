<template>
    <div class="progress-container" :class="[sizeClass, variantClass]">
        <!-- Linear Progress Bar -->
        <div v-if="variant === 'linear'" class="progress-linear">
            <div class="progress-track">
                <div 
                    class="progress-fill" 
                    :style="{ width: `${percentage}%` }"
                ></div>
            </div>
            <div v-if="showLabel" class="progress-label">{{ label }}</div>
        </div>

        <!-- Circle Progress Bar -->
        <div v-else-if="variant === 'circle'" class="progress-circle">
            <svg :width="size" :height="size" viewBox="0 0 40 40">
                <!-- Track -->
                <circle 
                    class="progress-track-circle" 
                    cx="20" 
                    cy="20" 
                    r="16" 
                    :stroke="trackColor"
                    :stroke-width="strokeWidth"
                    fill="none"
                />
                
                <!-- Progress -->
                <circle 
                    class="progress-fill-circle" 
                    cx="20" 
                    cy="20" 
                    r="16" 
                    :stroke="color"
                    :stroke-width="strokeWidth"
                    fill="none"
                    stroke-linecap="round"
                    :stroke-dasharray="circumference"
                    :stroke-dashoffset="dashOffset"
                />
            </svg>
            <div v-if="showLabel" class="progress-label-circle">{{ label }}</div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
    modelValue?: number;
    min?: number;
    max?: number;
    variant?: 'linear' | 'circle';
    size?: 'small' | 'medium' | 'large';
    color?: string;
    trackColor?: string;
    strokeWidth?: number;
    showLabel?: boolean;
    label?: string;
}

const props = withDefaults(defineProps<Props>(), {
    modelValue: 0,
    min: 0,
    max: 100,
    variant: 'linear',
    size: 'medium',
    color: '#4285f4',
    trackColor: '#55575C',
    strokeWidth: 4,
    showLabel: true,
    label: 'Progress Bar'
});

const sizeClass = computed(() => `progress-${props.size}`);
const variantClass = computed(() => `progress-${props.variant}`);

const percentage = computed(() => {
    const range = props.max - props.min;
    const value = props.modelValue - props.min;
    return Math.min(Math.max((value / range) * 100, 0), 100);
});

const size = computed(() => {
    switch (props.size) {
        case 'small': return 40;
        case 'large': return 120;
        default: return 80;
    }
});

const circumference = computed(() => {
    const radius = 16;
    return 2 * Math.PI * radius;
});

const dashOffset = computed(() => {
    const progress = percentage.value / 100;
    return circumference.value - (progress * circumference.value);
});
</script>

<style scoped>
.progress-container {
    position: relative;
    display: inline-block;
}

/* Linear Progress */
.progress-linear {
    width: 100%;
}

.progress-track {
    width: 100%;
    height: 8px;
    background-color: v-bind('props.trackColor');
    border-radius: 4px;
    overflow: hidden;
    position: relative;
}

.progress-fill {
    height: 100%;
    background-color: v-bind('props.color');
    border-radius: 4px;
    transition: width 0.3s ease;
}

.progress-label {
    text-align: center;
    margin-top: 8px;
    color: #ffffff;
    font-size: 14px;
    font-weight: 500;
}

/* Circle Progress */
.progress-circle {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.progress-track-circle {
    /* Track styles */
}

.progress-fill-circle {
    transition: stroke-dashoffset 0.3s ease;
    transform: rotate(-90deg);
    transform-origin: center;
}

.progress-label-circle {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: #ffffff;
    font-size: 12px;
    font-weight: 500;
    text-align: center;
}

/* Sizes */
.progress-small {
    width: 200px;
}

.progress-small .progress-circle {
    width: 40px;
    height: 40px;
}

.progress-medium {
    width: 300px;
}

.progress-medium .progress-circle {
    width: 80px;
    height: 80px;
}

.progress-large {
    width: 400px;
}

.progress-large .progress-circle {
    width: 120px;
    height: 120px;
}

/* Variants */
.progress-linear .progress-track {
    height: 6px;
}

.progress-linear .progress-fill {
    height: 6px;
}

.progress-circle .progress-track-circle {
    stroke-width: v-bind('props.strokeWidth');
}

.progress-circle .progress-fill-circle {
    stroke-width: v-bind('props.strokeWidth');
}
</style>
