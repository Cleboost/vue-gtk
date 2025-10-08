<template>
    <div 
        class="row-switch row-base" 
        :class="{ 'last-item': isLast }"
    >
        <div class="row-content">
            <span class="row-text">{{ text }}</span>
            <span v-if="subtitle" class="row-subtitle">{{ subtitle }}</span>
        </div>
        <Switch 
            v-model="switchValue" 
            :size="switchSize"
        />
    </div>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue';
import Switch from '../switch.vue';

interface Props {
    text: string;
    subtitle?: string;
    isLast?: boolean;
    modelValue?: boolean;
    switchSize?: 'small' | 'medium' | 'large';
}

const props = withDefaults(defineProps<Props>(), {
    modelValue: false,
    switchSize: 'medium'
});

const emit = defineEmits<{
    'update:modelValue': [value: boolean];
}>();

const switchValue = ref(props.modelValue);

watch(() => props.modelValue, (newValue) => {
    switchValue.value = newValue;
});

watch(switchValue, (newValue) => {
    emit('update:modelValue', newValue);
});
</script>

<style scoped>
@import '../../css/row.css';
</style>

