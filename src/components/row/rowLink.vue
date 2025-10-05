<template>
    <a 
        class="row-link" 
        :class="{ 'last-item': isLast }"
        :href="href"
        target="_blank"
    >
        <div class="row-content">
            <span class="row-text">{{ text }}</span>
            <span v-if="subtitle" class="row-subtitle">{{ subtitle }}</span>
        </div>
        <Icon :icon="linkIcon" />
    </a>
</template>

<script setup lang="ts">
import { Icon } from '@iconify/vue';
import { computed } from 'vue';

interface Props {
    text: string;
    href: string;
    subtitle?: string;
    isLast?: boolean;
}

const props = defineProps<Props>();

const linkIcon = computed(() => {
    const url = props.href.toLowerCase();
    
    if (url.includes('github.com')) {
        return 'tabler:brand-github';
    } else if (url.includes('gitlab.com')) {
        return 'tabler:brand-gitlab';
    } else {
        return 'tabler:external-link';
    }
});
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cantarell:ital,wght@0,400;0,700;1,400;1,700&display=swap');

.row-link {
    width: 100%;
    background-color: #33353B;
    border: none;
    padding: 16px 20px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    cursor: pointer;
    transition: background-color 0.2s ease;
    font-family: 'Cantarell', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    text-decoration: none;
    color: inherit;
    box-sizing: border-box;
    overflow: hidden;
}

.row-link:not(.last-item) {
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.row-link.last-item {
    border-bottom: none;
}

.row-link:hover {
    background-color: #393B3F;
}

.row-link:active {
    background-color: rgba(255, 255, 255, 0.1);
}

.row-content {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 2px;
    flex: 1;
    min-width: 0;
}

.row-text {
    color: #ffffff;
    font-size: 16px;
    font-weight: 400;
    text-align: left;
}

.row-subtitle {
    color: #9ca3af;
    font-size: 14px;
    font-weight: 400;
    text-align: left;
}

.row-link :deep(.iconify) {
    color: #ffffff;
    font-size: 18px;
    flex-shrink: 0;
    margin-left: 12px;
}
</style>
