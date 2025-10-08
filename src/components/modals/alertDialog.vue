<template>
    <Modal 
        :is-visible="isVisible"
        :show-close-button="false"
        :close-on-overlay="closeOnOverlay"
        @close="handleCancel"
    >
        <div class="alert-dialog-content">
            <h3 class="dialog-title">{{ title }}</h3>
            <p class="dialog-description">{{ description }}</p>
            
            <div class="dialog-actions">
                <Button 
                    variant="secondary" 
                    @click="handleCancel"
                >
                    {{ cancelText }}
                </Button>
                <Button 
                    variant="danger" 
                    @click="handleConfirm"
                >
                    {{ confirmText }}
                </Button>
            </div>
        </div>
    </Modal>
</template>

<script setup lang="ts">
import Modal from './modal.vue';
import Button from '../button.vue';

interface Props {
    isVisible: boolean;
    title?: string;
    description?: string;
    cancelText?: string;
    confirmText?: string;
    closeOnOverlay?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
    title: 'Are you sure?',
    description: 'Are you sure you want to do this? This action cannot be undone.',
    cancelText: 'Cancel',
    confirmText: 'Confirm',
    closeOnOverlay: true
});

const emit = defineEmits<{
    close: [];
    cancel: [];
    confirm: [];
}>();

const handleCancel = () => {
    emit('close');
    emit('cancel');
};

const handleConfirm = () => {
    emit('close');
    emit('confirm');
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cantarell:ital,wght@0,400;0,700;1,400;1,700&display=swap');

.alert-dialog-content {
    padding: 8px;
    font-family: 'Cantarell', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    min-width: 300px;
    max-width: 450px;
}

.dialog-title {
    margin: 0 0 12px 0;
    font-size: 20px;
    font-weight: 700;
    color: #FFFFFF;
}

.dialog-description {
    margin: 0 0 24px 0;
    font-size: 14px;
    line-height: 1.5;
    color: #B0B0B0;
}

.dialog-actions {
    display: flex;
    gap: 12px;
    justify-content: flex-end;
}
</style>

