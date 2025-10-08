<template>
    <div class="textarea-container" :class="sizeClass">
        <textarea
            :value="modelValue"
            @input="handleInput"
            @keydown="handleKeydown"
            :placeholder="placeholder"
            :disabled="disabled"
            :readonly="readonly"
            :rows="rows"
            :maxlength="maxlength"
            class="textarea-field"
        ></textarea>
    </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
    modelValue?: string;
    placeholder?: string;
    disabled?: boolean;
    readonly?: boolean;
    size?: 'small' | 'medium' | 'large';
    rows?: number;
    maxlength?: number;
}

const props = withDefaults(defineProps<Props>(), {
    modelValue: '',
    placeholder: 'Multiline Text',
    disabled: false,
    readonly: false,
    size: 'medium',
    rows: 4,
    maxlength: undefined
});

const emit = defineEmits<{
    'update:modelValue': [value: string];
    'enter': [event: KeyboardEvent];
}>();

const sizeClass = computed(() => `textarea-${props.size}`);

const handleInput = (event: Event) => {
    const target = event.target as HTMLTextAreaElement;
    emit('update:modelValue', target.value);
};

const handleKeydown = (event: KeyboardEvent) => {
    if (event.key === 'Enter' && !event.shiftKey) {
        emit('enter', event);
    }
};
</script>

<style scoped>
.textarea-container {
    position: relative;
    display: inline-block;
    width: 100%;
}

.textarea-field {
    width: 100%;
    padding: 12px 16px;
    background-color: #33353B;
    border: 1px solid #55575C;
    border-radius: 8px;
    color: #ffffff;
    font-family: 'Cantarell', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    font-size: 14px;
    line-height: 1.5;
    resize: vertical;
    outline: none;
    transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
    box-sizing: border-box;
}

.textarea-field::placeholder {
    color: #888888;
}

.textarea-field:focus {
    border-color: #4285f4;
    box-shadow: 0 0 0 2px rgba(66, 133, 244, 0.2);
}

.textarea-field:disabled {
    background-color: #2a2b2f;
    color: #666666;
    cursor: not-allowed;
}

.textarea-field:read-only {
    background-color: #2a2b2f;
    cursor: default;
}

/* Sizes */
.textarea-small {
    font-size: 12px;
}

.textarea-small .textarea-field {
    padding: 8px 12px;
}

.textarea-medium {
    font-size: 14px;
}

.textarea-medium .textarea-field {
    padding: 12px 16px;
}

.textarea-large {
    font-size: 16px;
}

.textarea-large .textarea-field {
    padding: 16px 20px;
}
</style>
