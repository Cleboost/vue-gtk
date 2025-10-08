<template>
    <div class="input-wrapper" :class="[sizeClass, { 'has-icon-left': type === 'search', 'has-icon-right': type === 'password' }]">
        <FolderSavedSearchSymbolic v-if="type === 'search'" icon="tabler:search" class="input-icon input-icon-left" />
        <input 
            :type="currentType"
            :placeholder="placeholder"
            v-model="inputValue"
            :disabled="disabled"
            :readonly="readonly"
            :class="inputClass"
            @focus="handleFocus"
            @blur="handleBlur"
            @keydown="handleKeydown"
        />
         <DialogPasswordSymbolic v-if="type === 'password'" class="input-icon input-icon-right" @click="togglePasswordVisibility" />
    </div>
</template>

<script setup lang="ts">
import { computed, ref, watch } from 'vue';
import { Icon } from '@iconify/vue';
import { FolderSavedSearchSymbolic, DialogPasswordSymbolic } from 'symbolic-icons';

interface Props {
    modelValue?: string;
    type?: 'text' | 'email' | 'password' | 'number' | 'search';
    placeholder?: string;
    disabled?: boolean;
    readonly?: boolean;
    size?: 'small' | 'medium' | 'large';
}

const props = withDefaults(defineProps<Props>(), {
    modelValue: '',
    type: 'text',
    placeholder: '',
    disabled: false,
    readonly: false,
    size: 'medium'
});

const emit = defineEmits<{
    'update:modelValue': [value: string];
    focus: [event: FocusEvent];
    blur: [event: FocusEvent];
    enter: [event: KeyboardEvent];
}>();

const showPassword = ref(false);
const localValue = ref(props.modelValue);

const sizeClass = computed(() => `input-${props.size}`);

const inputClass = computed(() => [
    'gtk-input',
    {
        'input-disabled': props.disabled,
        'input-readonly': props.readonly
    }
]);

const currentType = computed(() => {
    if (props.type === 'password') {
        return showPassword.value ? 'text' : 'password';
    }
    return props.type;
});

const inputValue = computed({
    get: () => localValue.value,
    set: (value: string) => {
        localValue.value = value;
        emit('update:modelValue', value);
    }
});

const handleInput = (event: Event) => {
    const target = event.target as HTMLInputElement;
    inputValue.value = target.value;
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

const togglePasswordVisibility = () => {
    showPassword.value = !showPassword.value;
};

watch(() => props.modelValue, (newValue) => {
    localValue.value = newValue;
});
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cantarell:ital,wght@0,400;0,700;1,400;1,700&display=swap');

.input-wrapper {
    position: relative;
    display: inline-flex;
    align-items: center;
    background-color: #26282D;
    border-radius: 8px;
    border: 1px solid transparent;
    transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
    width: 100%;
}

.input-wrapper:focus-within {
    border-color: #4285f4;
    box-shadow: 0 0 0 2px rgba(66, 133, 244, 0.2);
}

.gtk-input {
    width: 100%;
    background-color: #2D2F34;
    border: 1px solid #55575C;
    border-radius: 6px;
    color: #FFFFFF;
    font-family: 'Cantarell', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    font-weight: 400;
    outline: none;
    transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
    box-sizing: border-box;
}

.gtk-input::placeholder {
    color: #888888;
}

.gtk-input:focus {
    border-color: #4285f4;
    box-shadow: 0 0 0 2px rgba(66, 133, 244, 0.2);
}

.gtk-input:disabled {
    opacity: 0.6;
    cursor: not-allowed;
    background-color: #1A1B1F;
}

.gtk-input:read-only {
    background-color: #1A1B1F;
    cursor: default;
}

.input-icon {
    position: absolute;
    color: #888888;
    font-size: 18px;
    z-index: 1;
    pointer-events: none;
}

.input-icon-left {
    left: 12px;
}

.input-icon-right {
    right: 12px;
    cursor: pointer;
    pointer-events: auto;
}

.input-icon-right:hover {
    color: #FFFFFF;
}

.has-icon-left .gtk-input {
    padding-left: 40px;
}

.has-icon-right .gtk-input {
    padding-right: 40px;
}

.input-small {
    min-height: 32px;
}

.input-small .gtk-input {
    padding: 6px 12px;
    font-size: 14px;
}

.input-small .input-icon {
    font-size: 16px;
}

.input-small.has-icon-left .gtk-input {
    padding-left: 36px;
}

.input-small.has-icon-right .gtk-input {
    padding-right: 36px;
}

.input-medium {
    min-height: 40px;
}

.input-medium .gtk-input {
    padding: 8px 16px;
    font-size: 16px;
}

.input-medium .input-icon {
    font-size: 18px;
}

.input-medium.has-icon-left .gtk-input {
    padding-left: 40px;
}

.input-medium.has-icon-right .gtk-input {
    padding-right: 40px;
}

.input-large {
    min-height: 48px;
}

.input-large .gtk-input {
    padding: 12px 20px;
    font-size: 18px;
}

.input-large .input-icon {
    font-size: 20px;
}

.input-large.has-icon-left .gtk-input {
    padding-left: 44px;
}

.input-large.has-icon-right .gtk-input {
    padding-right: 44px;
}
</style>
