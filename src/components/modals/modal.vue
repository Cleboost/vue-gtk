<template>
    <Transition name="modal-overlay" appear>
        <div v-if="isVisible" class="modal-overlay" @click="handleOverlayClick">
            <Transition name="modal-content" appear>
                <div class="modal-container" @click.stop>
                    <button 
                        v-if="showCloseButton" 
                        class="modal-close-button" 
                        @click="handleClose"
                    >
                        <Icon icon="tabler:x" />
                    </button>
                    <slot></slot>
                </div>
            </Transition>
        </div>
    </Transition>
</template>

<script setup lang="ts">
import { Icon } from '@iconify/vue';

interface Props {
    isVisible: boolean;
    showCloseButton?: boolean;
    closeOnOverlay?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
    showCloseButton: true,
    closeOnOverlay: true
});

const emit = defineEmits<{
    close: [];
}>();

const handleClose = () => {
    emit('close');
};

const handleOverlayClick = () => {
    if (props.closeOnOverlay) {
        emit('close');
    }
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cantarell:ital,wght@0,400;0,700;1,400;1,700&display=swap');

.modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.6);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
    font-family: 'Cantarell', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}

.modal-container {
    background-color: #26282D;
    border: 1px solid #35353B;
    border-radius: 12px;
    box-shadow: 0 4px 16px rgba(0, 0, 0, 0.3);
    width: 90%;
    max-width: 500px;
    color: white;
    position: relative;
    padding: 20px;
    display: flex;
    flex-direction: column;
    gap: 20px;
}

.modal-close-button {
    position: absolute;
    top: 15px;
    right: 15px;
    background: none;
    border: none;
    color: #ffffff;
    font-size: 24px;
    cursor: pointer;
    padding: 5px;
    line-height: 1;
    transition: color 0.2s ease;
    z-index: 10;
}

.modal-close-button:hover {
    color: #ccc;
}

.modal-close-button :deep(.iconify) {
    font-size: 24px;
}

/* Animations */
.modal-overlay-enter-active,
.modal-overlay-leave-active {
    transition: opacity 0.3s ease;
}

.modal-overlay-enter-from,
.modal-overlay-leave-to {
    opacity: 0;
}

.modal-overlay-enter-to,
.modal-overlay-leave-from {
    opacity: 1;
}

.modal-content-enter-active,
.modal-content-leave-active {
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.modal-content-enter-from {
    opacity: 0;
    transform: scale(0.9) translateY(-20px);
}

.modal-content-leave-to {
    opacity: 0;
    transform: scale(0.9) translateY(20px);
}

.modal-content-enter-to,
.modal-content-leave-from {
    opacity: 1;
    transform: scale(1) translateY(0);
}
</style>
