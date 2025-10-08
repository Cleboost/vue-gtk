<template>
    <button 
        class="gtk-button" 
        :class="[sizeClass, variantClass, roundnessClass]"
        :disabled="disabled"
        @click="$emit('click', $event)"
    >
        <slot />
    </button>
</template>

<script setup lang="ts">
import { computed } from "vue";

interface Props {
    size?: 'small' | 'medium' | 'large';
    variant?: 'primary' | 'secondary' | 'outline' | 'danger';
    roundness?: 'square' | 'rounded' | 'pill';
    disabled?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
    size: 'medium',
    variant: 'primary',
    roundness: 'pill',
    disabled: false
});

const emit = defineEmits<{
    click: [event: MouseEvent];
}>();

const sizeClass = computed(() => `button-${props.size}`);
const variantClass = computed(() => `button-${props.variant}`);
const roundnessClass = computed(() => `button-${props.roundness}`);
</script>

<style scoped>
.gtk-button {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    border: none;
    font-family: 'Cantarell', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    font-weight: 500;
    transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
    user-select: none;
    text-decoration: none;
    outline: none;
    cursor: pointer;
}

.gtk-button:focus-visible {
    outline: 2px solid #4285f4;
    outline-offset: 2px;
}

.gtk-button:disabled {
    opacity: 0.6;
    cursor: not-allowed;
}

.button-small {
    padding: 6px 16px;
    font-size: 14px;
    min-height: 32px;
}

.button-medium {
    padding: 8px 24px;
    font-size: 16px;
    min-height: 40px;
}

.button-large {
    padding: 10px 32px;
    font-size: 18px;
    min-height: 48px;
}

.button-primary {
    background-color: #3482E0;
    color: white;
}

.button-primary:hover:not(:disabled) {
    background-color: #488EE3;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.15);
}

.button-primary:active:not(:disabled) {
    background-color: #2c5bb5;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.button-secondary {
    background-color: #494A50;
    color: #F4F5F6;
}

.button-secondary:hover:not(:disabled) {
    background-color: #595A60;
    border-color: #595A60;
}

.button-outline {
    background-color: transparent;
    color: #3482E0;
    border: 2px solid #3482E0;
}

.button-outline:hover:not(:disabled) {
    background-color: #488EE3;
    color: white;
}

.button-danger {
    background-color: #5B4549;
    color: #F6928D;
}

.button-danger:hover:not(:disabled) {
    background-color: #6B5559;
    box-shadow: 0 2px 6px rgba(91, 69, 73, 0.3);
}

.button-danger:active:not(:disabled) {
    background-color: #4B3539;
    box-shadow: 0 1px 3px rgba(91, 69, 73, 0.2);
}

.button-square {
    border-radius: 6px;
}

.button-rounded {
    border-radius: 12px;
}

.button-pill {
    border-radius: 30px;
}
</style>
