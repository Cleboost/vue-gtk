<template>
    <Modal 
        :is-visible="isVisible" 
        :show-close-button="true"
        :close-on-overlay="true"
        @close="$emit('close')"
    >
        <div class="shortcut-dialog">
            <div class="shortcut-header">
                <Input 
                    v-model="searchQuery"
                    type="search"
                    placeholder="Search shortcuts..."
                    size="small"
                    class="shortcut-search"
                />
            </div>
            
            <div class="shortcut-content">
                <div 
                    v-for="(category, categoryIndex) in filteredShortcuts" 
                    :key="categoryIndex"
                    class="shortcut-category"
                >
                    <h3 class="category-title">{{ category.name }}</h3>
                    <div class="category-content">
                        <RowShortcut
                            v-for="(shortcut, shortcutIndex) in category.content"
                            :key="shortcutIndex"
                            :title="shortcut.name"
                            :keys="shortcut.content"
                            :is-last="shortcutIndex === category.content.length - 1"
                        />
                    </div>
                </div>
            </div>
        </div>
    </Modal>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue';
import Modal from './modal.vue';
import RowShortcut from '../row/rowShortcut.vue';
import Input from '../input.vue';

interface ShortcutItem {
    name: string;
    content: string[];
}

interface ShortcutCategory {
    name: string;
    content: ShortcutItem[];
}

interface Props {
    isVisible: boolean;
    shortcuts: ShortcutCategory[];
}

const props = defineProps<Props>();

const emit = defineEmits<{
    close: [];
}>();

const searchQuery = ref('');

const filteredShortcuts = computed(() => {
    if (!searchQuery.value.trim()) {
        return props.shortcuts;
    }
    
    const query = searchQuery.value.toLowerCase();
    
    return props.shortcuts.map(category => ({
        ...category,
        content: category.content.filter(shortcut => 
            shortcut.name.toLowerCase().includes(query) ||
            shortcut.content.some(key => key.toLowerCase().includes(query))
        )
    })).filter(category => category.content.length > 0);
});
</script>

<style scoped>
.shortcut-dialog {
    width: 800px;
    max-width: 90vw;
    max-height: 80vh;
    overflow-y: auto;
}

.shortcut-header {
    padding: 16px 24px;
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    display: flex;
    justify-content: center;
}


.shortcut-content {
    padding: 20px 0;
}

.shortcut-category {
    margin-bottom: 32px;
}

.shortcut-category:last-child {
    margin-bottom: 0;
}

.category-title {
    color: #ffffff;
    font-size: 16px;
    font-weight: 600;
    margin: 0 0 12px 0;
    padding: 0 24px;
    font-family: 'Cantarell', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}

.category-content {
    background-color: #2a2b32;
    border-radius: 8px;
    overflow: hidden;
    margin: 0 24px;
}

.shortcut-search {
    max-width: 400px;
    margin: 0 auto;
}
</style>
