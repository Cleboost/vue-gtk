<template>
    <div class="slider-container" :class="[sizeClass, orientationClass]">
        <div class="slider-track" @click="handleTrackClick">
            <div 
                class="slider-fill" 
                :style="fillStyle"
            ></div>
            <div 
                class="slider-thumb" 
                :style="thumbStyle"
                @mousedown="startDrag"
                @touchstart="startDrag"
            ></div>
        </div>
        <input 
            ref="hiddenInput"
            type="range"
            :min="min"
            :max="max"
            :step="step"
            :value="modelValue"
            :disabled="disabled"
            class="slider-input"
            @input="handleInput"
            @change="handleChange"
        />
    </div>
</template>

<script setup lang="ts">
import { computed, ref, watch, onMounted, onUnmounted } from 'vue';

interface Props {
    modelValue?: number;
    min?: number;
    max?: number;
    step?: number;
    orientation?: 'horizontal' | 'vertical';
    size?: 'small' | 'medium' | 'large';
    disabled?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
    modelValue: 0,
    min: 0,
    max: 100,
    step: 1,
    orientation: 'horizontal',
    size: 'medium',
    disabled: false
});

const emit = defineEmits<{
    'update:modelValue': [value: number];
    change: [value: number];
}>();

const hiddenInput = ref<HTMLInputElement>();
const isDragging = ref(false);
const localValue = ref(props.modelValue);

const sizeClass = computed(() => `slider-${props.size}`);
const orientationClass = computed(() => `slider-${props.orientation}`);

const percentage = computed(() => {
    const range = props.max - props.min;
    return ((localValue.value - props.min) / range) * 100;
});

const fillStyle = computed(() => {
    const isVertical = props.orientation === 'vertical';
    return {
        [isVertical ? 'height' : 'width']: `${isVertical ? 100 - percentage.value : percentage.value}%`,
        [isVertical ? 'width' : 'height']: '100%',
        ...(isVertical && { bottom: '0' })
    };
});

const thumbStyle = computed(() => {
    const isVertical = props.orientation === 'vertical';
    return {
        [isVertical ? 'top' : 'left']: `${percentage.value}%`,
        [isVertical ? 'left' : 'top']: '50%',
        transform: isVertical ? 'translateY(-50%) translateX(-50%)' : 'translateX(-50%) translateY(-50%)'
    };
});

const handleInput = (event: Event) => {
    const newValue = parseFloat((event.target as HTMLInputElement).value);
    localValue.value = newValue;
    emit('update:modelValue', newValue);
};

const handleChange = (event: Event) => {
    emit('change', parseFloat((event.target as HTMLInputElement).value));
};

const handleTrackClick = (event: MouseEvent) => {
    if (props.disabled) return;
    
    const track = event.currentTarget as HTMLElement;
    const rect = track.getBoundingClientRect();
    const isVertical = props.orientation === 'vertical';
    const pos = isVertical ? event.clientY - rect.top : event.clientX - rect.left;
    const size = isVertical ? rect.height : rect.width;
    const newPercentage = (pos / size) * 100;
    const newValue = props.min + (newPercentage / 100) * (props.max - props.min);
    const clampedValue = Math.min(Math.max(newValue, props.min), props.max);
    
    localValue.value = clampedValue;
    emit('update:modelValue', clampedValue);
};

const startDrag = (event: MouseEvent | TouchEvent) => {
    if (props.disabled) return;
    
    event.preventDefault();
    isDragging.value = true;
    
    const handleMouseMove = (e: MouseEvent | TouchEvent) => {
        if (!isDragging.value) return;
        
        const track = hiddenInput.value?.parentElement?.querySelector('.slider-track') as HTMLElement;
        if (!track) return;
        
        const rect = track.getBoundingClientRect();
        const isVertical = props.orientation === 'vertical';
        const clientPos = e instanceof MouseEvent ? 
            (isVertical ? e.clientY : e.clientX) : 
            (isVertical ? e.touches[0].clientY : e.touches[0].clientX);
        const trackPos = isVertical ? rect.top : rect.left;
        const trackSize = isVertical ? rect.height : rect.width;
        const newPercentage = ((clientPos - trackPos) / trackSize) * 100;
        const newValue = props.min + (newPercentage / 100) * (props.max - props.min);
        const clampedValue = Math.min(Math.max(newValue, props.min), props.max);
        
        localValue.value = clampedValue;
        emit('update:modelValue', clampedValue);
    };
    
    const handleMouseUp = () => {
        isDragging.value = false;
        document.removeEventListener('mousemove', handleMouseMove);
        document.removeEventListener('mouseup', handleMouseUp);
        document.removeEventListener('touchmove', handleMouseMove);
        document.removeEventListener('touchend', handleMouseUp);
    };
    
    document.addEventListener('mousemove', handleMouseMove);
    document.addEventListener('mouseup', handleMouseUp);
    document.addEventListener('touchmove', handleMouseMove);
    document.addEventListener('touchend', handleMouseUp);
};

watch(() => props.modelValue, (newValue) => {
    localValue.value = newValue;
});
</script>

<style scoped>
.slider-container {
    position: relative;
    display: flex;
    font-family: 'Cantarell', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}

.slider-horizontal {
    flex-direction: row;
}

.slider-vertical {
    flex-direction: column;
}

.slider-horizontal {
    width: 200px;
    height: 20px;
}

.slider-vertical {
    width: 20px;
    height: 250px;
}

.slider-track {
    position: relative;
    background-color: #55575C;
    border-radius: 2px;
    cursor: pointer;
    transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

.slider-horizontal .slider-track {
    width: 100%;
    height: 3px;
}

.slider-vertical .slider-track {
    width: 3px;
    flex: 1;
}

.slider-fill {
    position: absolute;
    background-color: #4285f4;
    border-radius: 2px;
    transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
    z-index: 1;
}

.slider-thumb {
    position: absolute;
    width: 15px;
    height: 15px;
    background-color: #55575C;
    border-radius: 50%;
    cursor: grab;
    transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
    z-index: 2;
    border: 2px solid #2D2F34;
    box-sizing: border-box;
}

.slider-thumb:hover {
    background-color: #666;
    transform: scale(1.05);
}

.slider-thumb:active {
    cursor: grabbing;
    transform: scale(1.1);
    background-color: #777;
}

.slider-input {
    position: absolute;
    opacity: 0;
    pointer-events: none;
    width: 100%;
    height: 100%;
}

.slider-container:focus-within .slider-track {
    box-shadow: 0 0 0 2px rgba(66, 133, 244, 0.2);
}

.slider-container:disabled .slider-track {
    opacity: 0.6;
    cursor: not-allowed;
}

.slider-container:disabled .slider-thumb {
    cursor: not-allowed;
}

/* Sizes */
.slider-small .slider-horizontal {
    width: 150px;
    height: 16px;
}

.slider-small .slider-vertical {
    width: 16px;
    height: 150px;
}

.slider-small .slider-track {
    height: 2px;
}

.slider-small .slider-vertical .slider-track {
    width: 2px;
}

.slider-small .slider-thumb {
    width: 12px;
    height: 12px;
}

.slider-medium .slider-horizontal {
    width: 200px;
    height: 20px;
}

.slider-medium .slider-vertical {
    width: 20px;
    height: 200px;
}

.slider-medium .slider-track {
    height: 3px;
}

.slider-medium .slider-vertical .slider-track {
    width: 3px;
}

.slider-medium .slider-thumb {
    width: 15px;
    height: 15px;
}

.slider-large .slider-horizontal {
    width: 250px;
    height: 24px;
}

.slider-large .slider-vertical {
    width: 24px;
    height: 300px;
}

.slider-large .slider-track {
    height: 4px;
}

.slider-large .slider-vertical .slider-track {
    width: 4px;
}

.slider-large .slider-thumb {
    width: 18px;
    height: 18px;
}
</style>
