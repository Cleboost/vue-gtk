<template>
    <div class="about-credits">
        <div class="credits-header">
            <button class="back-button" @click="$emit('close')">
                <Icon icon="tabler:arrow-left" />
            </button>
            <h2 class="credits-title">Crédits</h2>
        </div>
        
        <div class="credits-content">
            <div v-for="(section, index) in credit" :key="index" class="credit-section">
                <h3 class="section-title">{{ section.name }}</h3>
                <div class="section-content">
                    <div v-for="(item, itemIndex) in section.content" :key="itemIndex" class="credit-item">
                        <span v-if="item.type === 'text'" class="credit-text">{{ item.texte }}</span>
                        <BoxRow v-else-if="item.type === 'link' && item.link">
                            <RowLink :text="item.texte" :href="item.link" :is-last="true" />
                        </BoxRow>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { Icon } from '@iconify/vue';
import BoxRow from '../../row/boxRow.vue';
import RowLink from '../../row/rowLink.vue';

interface CreditItem {
    type: 'text' | 'link';
    texte: string;
    link?: string;
}

interface CreditSection {
    name: string;
    content: CreditItem[];
}

interface Props {
    credit: CreditSection[];
}

defineProps<Props>();

defineEmits<{
    close: [];
}>();
</script>

<style scoped>
@import '../../../css/row.css';

.about-credits {
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

.credits-header {
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

.credits-title {
    color: #ffffff;
    font-size: 20px;
    font-weight: 600;
    margin: 0;
}

.credits-content {
    display: flex;
    flex-direction: column;
    gap: 16px;
}

.credit-section {
    display: flex;
    flex-direction: column;
    gap: 12px;
}

.section-title {
    color: #ffffff;
    font-size: 16px;
    font-weight: 600;
    margin: 0;
}

.section-content {
    display: flex;
    flex-direction: column;
    gap: 12px;
}

.credit-item {
    display: flex;
    flex-direction: column;
}

.credit-text {
    color: #ffffff;
    font-size: 14px;
    line-height: 1.4;
    margin-bottom: 8px;
}
</style>
