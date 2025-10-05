<template>
    <div class="about-remerciment">        
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
    padding: 20px 0 0 0;
    display: flex;
    flex-direction: column;
    gap: 16px;
    z-index: 10;
    min-height: 500px;
    box-sizing: border-box;
    overflow-y: auto;
    max-height: 70vh;
}

.remerciment-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 8px;
    margin-top: -35px;
    padding-top: 0;
    padding-right: 50px;
    flex-wrap: nowrap;
    position: relative;
    height: 40px;
}

.remerciment-title {
    color: #ffffff;
    font-size: 20px;
    font-weight: 600;
    margin: 0;
    padding: 0;
    flex: 1;
    text-align: center;
    line-height: 1;
    display: flex;
    align-items: center;
    justify-content: center;
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

.spacer {
    width: 36px;
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
