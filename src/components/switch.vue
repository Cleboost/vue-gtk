<template>
    <div class="toggle-container-wrapper">
        <label class="toggle-switch-container" :class="sizeClass">
            <input type="checkbox" v-model="isChecked" />
            <span class="slider" :class="{ checked: isChecked }"></span>
        </label>
    </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";

interface Props {
    size?: 'small' | 'medium' | 'large';
    modelValue?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
    size: 'medium',
    modelValue: true
});

const emit = defineEmits<{
    'update:modelValue': [value: boolean];
}>();

const isChecked = computed({
    get: () => props.modelValue,
    set: (value: boolean) => emit('update:modelValue', value)
});

const sizeClass = computed(() => `switch-${props.size}`);
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cantarell:ital,wght@0,400;0,700;1,400;1,700&display=swap');

:root {
    --toggle-blue: #4285f4;
    --toggle-grey: #5a5a5a;
    --toggle-disabled: #53555D;
    --thumb-disabled: #CCCED3;
}

.toggle-switch-container {
    position: relative;
    display: inline-flex;
    align-items: center;
    gap: 8px;
    cursor: pointer;
    user-select: none;
}

.switch-small {
    --toggle-width: 28px;
    --toggle-height: 16px;
    --thumb-size: 12px;
    --thumb-offset: 2px;
}

.switch-medium {
    --toggle-width: 36px;
    --toggle-height: 20px;
    --thumb-size: 16px;
    --thumb-offset: 2px;
}

.switch-large {
    --toggle-width: 44px;
    --toggle-height: 24px;
    --thumb-size: 20px;
    --thumb-offset: 2px;
}

.slider {
    position: relative;
    display: inline-block;
    width: var(--toggle-width);
    height: var(--toggle-height);
    background-color: #53555D;
    border-radius: calc(var(--toggle-height) / 2);
    transition: all 0.15s cubic-bezier(0.4, 0, 0.2, 1);
    box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.2);
}

.slider::before {
    content: "";
    position: absolute;
    left: var(--thumb-offset);
    top: 50%;
    transform: translateY(-50%);
    width: var(--thumb-size);
    height: var(--thumb-size);
    background-color: #CCCED3;
    border-radius: 50%;
    transition: all 0.15s cubic-bezier(0.4, 0, 0.2, 1);
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2), 0 1px 2px rgba(0, 0, 0, 0.1);
}

.toggle-switch-container input {
    opacity: 0;
    width: 0;
    height: 0;
}

.toggle-switch-container input:checked + .slider {
    background-color: #4285f4 !important;
    box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
}

.slider.checked {
    background-color: #4285f4 !important;
}

.toggle-switch-container input:not(:checked) + .slider {
    background-color: #53555D !important;
    box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.2);
}

.toggle-switch-container input:not(:checked) + .slider::before {
    background-color: #CCCED3 !important;
}

.toggle-switch-container input:checked + .slider::before {
    left: calc(var(--toggle-width) - var(--thumb-size) - var(--thumb-offset));
    background-color: white !important;
}


.toggle-switch-container input:focus + .slider {
    outline: 2px solid var(--toggle-blue);
    outline-offset: 2px;
}

.toggle-switch-container:hover .slider {
    box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.3);
}

.toggle-switch-container:hover input:checked + .slider {
    box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.2);
}
</style>
