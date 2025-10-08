<template>
    <div class="spin-button-container">
        <div class="spin-button" :class="sizeClass">
            <input 
                ref="inputRef"
                type="number"
                :value="displayValue"
                :min="min"
                :max="max"
                :step="step"
                :disabled="disabled"
                :readonly="readonly"
                class="spin-input"
                @input="handleInput"
                @focus="handleFocus"
                @blur="handleBlur"
                @keydown="handleKeydown"
            />
            <button 
                type="button"
                class="spin-button-decrement"
                :disabled="disabled || isAtMin"
                @click="decrement"
            >
                <Icon icon="tabler:minus" />
            </button>
            <button 
                type="button"
                class="spin-button-increment"
                :disabled="disabled || isAtMax"
                @click="increment"
            >
                <Icon icon="tabler:plus" />
            </button>
        </div>
    </div>
</template>

<script setup lang="ts">
import { computed, ref, watch } from 'vue';
import { Icon } from '@iconify/vue';

interface Props {
    modelValue?: number;
    min?: number;
    max?: number;
    step?: number;
    disabled?: boolean;
    readonly?: boolean;
    size?: 'small' | 'medium' | 'large';
}

const props = withDefaults(defineProps<Props>(), {
    modelValue: 0,
    min: -Infinity,
    max: Infinity,
    step: 1,
    disabled: false,
    readonly: false,
    size: 'medium'
});

const emit = defineEmits<{
    'update:modelValue': [value: number];
    focus: [event: FocusEvent];
    blur: [event: FocusEvent];
    enter: [event: KeyboardEvent];
}>();

const inputRef = ref<HTMLInputElement>();
const localValue = ref(props.modelValue);

const sizeClass = computed(() => `spin-${props.size}`);

const displayValue = computed(() => {
    return localValue.value;
});

const isAtMin = computed(() => {
    return localValue.value <= props.min;
});

const isAtMax = computed(() => {
    return localValue.value >= props.max;
});

const handleInput = (event: Event) => {
    const target = event.target as HTMLInputElement;
    const newValue = parseFloat(target.value) || 0;
    updateValue(newValue);
};

const handleFocus = (event: FocusEvent) => {
    emit('focus', event);
};

const handleBlur = (event: FocusEvent) => {
    emit('blur', event);
};

const handleKeydown = (event: KeyboardEvent) => {
    if (event.key === 'Enter') {
        emit('enter', event);
    }
};

const increment = () => {
    const newValue = Math.min(localValue.value + props.step, props.max);
    updateValue(newValue);
};

const decrement = () => {
    const newValue = Math.max(localValue.value - props.step, props.min);
    updateValue(newValue);
};

const updateValue = (newValue: number) => {
    const clampedValue = Math.min(Math.max(newValue, props.min), props.max);
    localValue.value = clampedValue;
    emit('update:modelValue', clampedValue);
};

watch(() => props.modelValue, (newValue) => {
    localValue.value = newValue;
});
</script>

<style scoped>
.spin-button-container {
    display: inline-block;
}

.spin-button {
    display: flex;
    background-color: #26282D;
    border: 1px solid #55575C;
    border-radius: 8px;
    overflow: hidden;
    transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
    font-family: 'Cantarell', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}

.spin-button:focus-within {
    border-color: #4285f4;
    box-shadow: 0 0 0 2px rgba(66, 133, 244, 0.2);
}

.spin-input {
    flex: 1;
    background-color: #2D2F34;
    border: none;
    color: #FFFFFF;
    text-align: center;
    outline: none;
    font-family: inherit;
    font-weight: 500;
    transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
    box-sizing: border-box;
    -moz-appearance: textfield;
}

.spin-input::-webkit-outer-spin-button,
.spin-input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
}

.spin-input:focus {
    background-color: #4285f4;
    color: #FFFFFF;
}

.spin-input:disabled {
    opacity: 0.6;
    cursor: not-allowed;
}

.spin-input:read-only {
    cursor: default;
}

.spin-button-decrement,
.spin-button-increment {
    background-color: #2D2F34;
    border: none;
    color: #FFFFFF;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
    border-left: 1px solid #55575C;
}

.spin-button-increment {
    border-left: 1px solid #55575C;
    border-right: none;
}

.spin-button-decrement:hover:not(:disabled),
.spin-button-increment:hover:not(:disabled) {
    background-color: #393B3F;
}

.spin-button-decrement:active:not(:disabled),
.spin-button-increment:active:not(:disabled) {
    background-color: #1A1B1F;
}

.spin-button-decrement:disabled,
.spin-button-increment:disabled {
    opacity: 0.4;
    cursor: not-allowed;
}

/* Sizes */
.spin-small {
    min-height: 32px;
}

.spin-small .spin-input {
    padding: 6px 12px;
    font-size: 14px;
}

.spin-small .spin-button-decrement,
.spin-small .spin-button-increment {
    width: 32px;
    font-size: 14px;
}

.spin-medium {
    min-height: 40px;
}

.spin-medium .spin-input {
    padding: 8px 16px;
    font-size: 16px;
}

.spin-medium .spin-button-decrement,
.spin-medium .spin-button-increment {
    width: 40px;
    font-size: 16px;
}

.spin-large {
    min-height: 48px;
}

.spin-large .spin-input {
    padding: 12px 20px;
    font-size: 18px;
}

.spin-large .spin-button-decrement,
.spin-large .spin-button-increment {
    width: 48px;
    font-size: 18px;
}
</style>
