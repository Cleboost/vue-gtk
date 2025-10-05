<template>
    <div class="about-credits">
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
    display: flex;
    flex-direction: column;
    gap: 16px;
    z-index: 10;
    min-height: 500px;
    box-sizing: border-box;
    overflow-y: auto;
    max-height: 70vh;
    padding: 20px 0 0 0;
}

.credits-header {
    text-align: center;
    margin-bottom: 8px;
    margin-top: -35px;
    padding-top: 0;
}

.credits-title {
    color: #ffffff;
    font-size: 20px;
    font-weight: 600;
    margin: 0;
    padding: 0;
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
