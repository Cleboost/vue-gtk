<template>
    <a 
        class="row-link row-base" 
        :class="{ 'last-item': isLast }"
        :href="href"
        target="_blank"
    >
        <div class="row-content">
            <span class="row-text">{{ text }}</span>
            <span v-if="subtitle" class="row-subtitle">{{ subtitle }}</span>
        </div>
        <Icon :icon="linkIcon" class="row-icon"/>
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
@import '../../css/row.css';
</style>
