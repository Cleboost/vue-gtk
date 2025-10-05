<template>
    <div class="about-remerciment">
        <div class="remerciment-header">
            <button class="back-button" @click="$emit('close')">
                <Icon icon="tabler:arrow-left" />
            </button>
            <h2 class="remerciment-title">Remerciements</h2>
        </div>
        
        <div class="remerciment-content">
            <BoxRow>
                <RowButton 
                    v-for="(item, index) in remerciement"
                    :key="item.name"
                    :text="item.name" 
                    :subtitle="item.quote"
                    :is-last="index === remerciement.length - 1"
                />
            </BoxRow>
        </div>
    </div>
</template>

<script setup lang="ts">
import { Icon } from '@iconify/vue';
import BoxRow from '../../row/boxRow.vue';
import RowButton from '../../row/rowButton.vue';

interface RemerciementItem {
    name: string;
    quote: string;
}

interface Props {
    remerciement: RemerciementItem[];
}

defineProps<Props>();

defineEmits<{
    close: [];
}>();
</script>

<style scoped>
@import '../../../css/row.css';

.about-remerciment {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #26282D;
    border-radius: 12px;
    padding: 20px;
    display: flex;
    flex-direction: column;
    gap: 16px;
    z-index: 10;
    min-height: 500px;
    box-sizing: border-box;
    overflow-y: auto;
    max-height: 70vh;
    transition: all 0.6s cubic-bezier(0.25, 0.46, 0.45, 0.94);
    transform-origin: center center;
}

.remerciment-header {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 8px;
}

.back-button {
    background: none;
    border: none;
    color: #ffffff;
    font-size: 20px;
    cursor: pointer;
    padding: 8px;
    border-radius: 50%;
    transition: background-color 0.2s ease;
    display: flex;
    align-items: center;
    justify-content: center;
}

.back-button:hover {
    background-color: rgba(255, 255, 255, 0.1);
}

.remerciment-title {
    color: #ffffff;
    font-size: 20px;
    font-weight: 600;
    margin: 0;
}

.remerciment-content {
    display: flex;
    flex-direction: column;
    gap: 16px;
}

.remerciment-item {
    margin-bottom: 16px;
}

.remerciment-item:last-child {
    margin-bottom: 0;
}
</style>
