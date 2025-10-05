<template>
    <Modal 
        :is-visible="isVisible"
        :show-close-button="showCloseButton"
        @close="closeModal"
    >
        <Transition name="header-fade">
            <div v-if="!showCredits && !showMentions && !showRemerciement" class="about-header">
                <div class="about-icon">
                    <Icon icon="tabler:database" />
                </div>
                <h1 class="about-title">{{ nom }}</h1>
                <p v-if="auteur" class="about-author">{{ auteur }}</p>
                <div class="version-badge">{{ version }}</div>
            </div>
        </Transition>
        
        <div class="about-content" :class="{ 'expanded': showCredits || showMentions || showRemerciement }">
            <div v-if="!showCredits && !showMentions && !showRemerciement" class="about-main">
                <BoxRow v-if="web">
                    <RowLink v-if="web" text="Site web" :href="web" :is-last="true" />
                </BoxRow>
                <BoxRow v-if="issue || depannage">
                    <RowLink
                        v-if="issue"
                        text="Signaler un problème"
                        :href="issue"
                        :is-last="!depannage"
                    />
                    <RowButton
                        v-if="depannage"
                        text="Dépannage"
                        @click="handleTroubleshooting"
                        :is-last="true"
                    />
                </BoxRow>
                <BoxRow v-if="credit && credit.length > 0 || mentionLegales || remerciement">
                    <RowButton v-if="credit && credit.length > 0" text="Crédits" @click="handleCredits" :is-last="!mentionLegales" />
                    <RowButton v-if="mentionLegales" text="Mentions légales" @click="handleLegal" :is-last="!remerciement"/>
                    <RowButton v-if="remerciement" text="Remerciements" @click="handleAcknowledgements" :is-last="true"/>
                </BoxRow>
            </div>
            
            <Transition name="overlay" appear>
                <Credit v-if="showCredits" key="credits" :credit="credit || []" @close="closeCredits" />
            </Transition>
            
            <Transition name="overlay" appear>
                <Mentions v-if="showMentions" key="mentions" :mention-legales="mentionLegales" @close="closeMentions" />
            </Transition>
            
            <Transition name="overlay" appear>
                <Remerciment v-if="showRemerciement" key="remerciment" :remerciement="remerciement" @close="closeRemerciement" />
            </Transition>
        </div>
    </Modal>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { Icon } from '@iconify/vue';
import Modal from './modal.vue';
import Credit from './about/credit.vue';
import Mentions from './about/mentions.vue';
import Remerciment from './about/remerciment.vue';
import BoxRow from '../row/boxRow.vue';
import RowButton from '../row/rowButton.vue';
import RowLink from '../row/rowLink.vue';

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
    isVisible: boolean;
    nom: string;
    auteur?: string;
    version: string;
    web?: string;
    issue?: string;
    depannage?: string;
    credit?: CreditSection[];
    mentionLegales?: string;
    remerciement?: string;
    showCloseButton?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
    isVisible: false,
    auteur: '',
    web: '',
    issue: '',
    depannage: '',
    credit: () => [],
    mentionLegales: '',
    remerciement: '',
    showCloseButton: true
});

const emit = defineEmits<{
    close: [];
    depannage: [];
    credit: [];
    mentionLegales: [];
    remerciement: [];
}>();

const showCredits = ref(false);
const showMentions = ref(false);
const showRemerciement = ref(false);

const closeModal = () => {
    emit('close');
};

const handleTroubleshooting = () => {
    emit('depannage');
};

const handleCredits = () => {
    showCredits.value = true;
};

const closeCredits = () => {
    showCredits.value = false;
};

const handleLegal = () => {
    showMentions.value = true;
};

const closeMentions = () => {
    showMentions.value = false;
};

const handleAcknowledgements = () => {
    showRemerciement.value = true;
};

const closeRemerciement = () => {
    showRemerciement.value = false;
};
</script>

<style scoped>
@import '../../css/row.css';


.about-header {
    text-align: center;
}

.about-icon {
    font-size: 128px;
    color: #4caf50;
}

.about-title {
    color: #ffffff;
    font-size: 32px;
    font-weight: 900;
    margin: 0 0 0 0;
}

.about-author {
    color: #ffffff;
    font-size: 20px;
    font-weight: 600;
    margin: 0 0 16px 0;
}

.version-badge {
    display: inline-block;
    background-color: #4285f4;
    color: #ffffff;
    padding: 4px 12px;
    border-radius: 12px;
    font-size: 14px;
    font-weight: 900;
}

.about-content {
    margin-top: 24px;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    position: relative;
    min-height: 200px;
    transition: all 0.6s cubic-bezier(0.25, 0.46, 0.45, 0.94);
    transform-origin: center center;
}

.about-content.expanded {
    min-height: 500px;
}

.about-content :deep(.box-row) {
    margin-bottom: 24px;
}

.about-content :deep(.box-row:last-child) {
    margin-bottom: 0;
}

/* Animation de superposition */
.overlay-enter-active,
.overlay-leave-active {
    transition: all 0.6s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

.overlay-enter-from {
    opacity: 0;
    transform: translateX(100%) scale(0.9);
}

.overlay-leave-to {
    opacity: 0;
    transform: translateX(100%) scale(0.9);
}

.overlay-enter-to,
.overlay-leave-from {
    opacity: 1;
    transform: translateX(0) scale(1);
}

/* Animation du header */
.header-fade-enter-active,
.header-fade-leave-active {
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.header-fade-enter-from,
.header-fade-leave-to {
    opacity: 0;
    transform: translateY(-20px);
}

.header-fade-enter-to,
.header-fade-leave-from {
    opacity: 1;
    transform: translateY(0);
}

</style>