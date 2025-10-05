<template>
    <div v-if="isVisible" class="context-menu-overlay" @click="closeMenu">
        <div 
            class="context-menu" 
            :style="{ left: getAdjustedPosition(props.position).x + 'px', top: getAdjustedPosition(props.position).y + 'px' }"
            @click.stop
        >
            <div 
                v-for="(item, index) in items" 
                :key="index"
                class="context-menu-item"
                :class="{ 'has-icon': item.icon, 'has-shortcut': item.shortcut }"
                @click="handleItemClick(item, index)"
            >
                <div class="item-content">
                    <span class="item-text">{{ item.text }}</span>
                    <span v-if="item.shortcut" class="item-shortcut">{{ item.shortcut }}</span>
                </div>
                <Icon v-if="item.icon" :icon="item.icon" class="item-icon" />
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { Icon } from '@iconify/vue';
import { onMounted, onUnmounted } from 'vue';

interface ContextMenuItem {
    text: string;
    shortcut?: string;
    icon?: string;
    action?: () => void;
}

interface Props {
    isVisible: boolean;
    items: ContextMenuItem[];
    position?: { x: number; y: number };
}

const props = withDefaults(defineProps<Props>(), {
    position: () => ({ x: 0, y: 0 })
});

const emit = defineEmits<{
    close: [];
    itemClick: [item: ContextMenuItem, index: number];
}>();

const closeMenu = () => {
    emit('close');
};

const handleItemClick = (item: ContextMenuItem, index: number) => {
    if (item.action) {
        item.action();
    }
    emit('itemClick', item, index);
    closeMenu();
};

const getAdjustedPosition = (position: { x: number; y: number }) => {
    const menuWidth = 250;
    const menuHeight = 200;
    const padding = 10;
    
    let adjustedX = position.x;
    let adjustedY = position.y;
    
    if (position.x + menuWidth > window.innerWidth - padding) {
        adjustedX = window.innerWidth - menuWidth - padding;
    }
    
    if (position.y + menuHeight > window.innerHeight - padding) {
        adjustedY = window.innerHeight - menuHeight - padding;
    }
    
    if (adjustedX < padding) {
        adjustedX = padding;
    }
    
    if (adjustedY < padding) {
        adjustedY = padding;
    }
    
    return { x: adjustedX, y: adjustedY };
};

const handleKeyDown = (event: KeyboardEvent) => {
    if (event.key === 'Escape') {
        closeMenu();
    }
};

onMounted(() => {
    document.addEventListener('keydown', handleKeyDown);
});

onUnmounted(() => {
    document.removeEventListener('keydown', handleKeyDown);
});
</script>

<style scoped>
.context-menu-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 1000;
    background: transparent;
}

.context-menu {
    position: fixed;
    background-color: #26282D;
    border-radius: 18px;
    box-shadow: 
        0 8px 32px rgba(0, 0, 0, 0.3),
        0 4px 16px rgba(0, 0, 0, 0.2);
    padding: 5px 5px;
    min-width: 200px;
    max-width: 300px;
    overflow: hidden;
    z-index: 1001;
}

.context-menu-item {
    display: flex;
    height: 7px;
    border-radius: 10px;
    align-items: center;
    justify-content: space-between;
    padding: 12px 16px;
    cursor: pointer;
    transition: background-color 0.2s ease;
    color: #ffffff;
    font-family: 'Cantarell', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    font-size: 14px;
    font-weight: 400;
}

.context-menu-item:hover {
    background-color: #33353B;
}

.item-content {
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex: 1;
    min-width: 0;
}

.item-text {
    color: #ffffff;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}

.item-shortcut {
    color: #ffffff;
    font-size: 12px;
    opacity: 0.7;
    margin-left: 16px;
    white-space: nowrap;
}

.item-icon {
    color: #ffffff;
    font-size: 16px;
    margin-left: 12px;
    flex-shrink: 0;
}

.context-menu-item.has-icon .item-content {
    margin-right: 8px;
}

.context-menu-item.has-shortcut .item-text {
    margin-right: 16px;
}

.context-menu-pointer {
    position: absolute;
    width: 0;
    height: 0;
    border: 8px solid transparent;
    z-index: 1;
}

.context-menu-pointer.pointer-left {
    left: -8px;
    border-right-color: #26282D;
}

.context-menu-pointer.pointer-right {
    right: -8px;
    border-left-color: #26282D;
}

.context-menu-pointer.pointer-top {
    top: -8px;
    border-bottom-color: #26282D;
}

.context-menu-pointer.pointer-bottom {
    bottom: -8px;
    border-top-color: #26282D;
}
</style>
