<template>
    <Modal 
        :is-visible="isVisible"
        :show-close-button="showCloseButton"
        @close="closeModal"
    >
        <div class="about-header">
            <div class="about-icon">
                <Icon icon="tabler:database" />
            </div>
            <h1 class="about-title">{{ nom }}</h1>
            <p v-if="auteur" class="about-author">{{ auteur }}</p>
            <div class="version-badge">{{ version }}</div>
        </div>
        
        <div class="about-content">
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
            <BoxRow v-if="credit || mentionLegales || remerciement">
                <RowButton v-if="credit" text="Crédits" @click="handleCredits" :is-last="!mentionLegales" />
                <RowButton v-if="mentionLegales" text="Mentions légales" @click="handleLegal" :is-last="!remerciement"/>
                <RowButton v-if="remerciement" text="Remerciements" @click="handleAcknowledgements" :is-last="true"/>
            </BoxRow>
        </div>
    </Modal>
</template>

<script setup lang="ts">
import { Icon } from '@iconify/vue';
import Modal from './modal.vue';
import BoxRow from '../row/boxRow.vue';
import RowButton from '../row/rowButton.vue';
import RowLink from '../row/rowLink.vue';

interface Props {
    isVisible: boolean;
    nom: string;
    auteur?: string;
    version: string;
    web?: string;
    issue?: string;
    depannage?: string;
    credit?: string;
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
    credit: '',
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

const closeModal = () => {
    emit('close');
};

const handleTroubleshooting = () => {
    emit('depannage');
};

const handleCredits = () => {
    emit('credit');
};

const handleLegal = () => {
    emit('mentionLegales');
};

const handleAcknowledgements = () => {
    emit('remerciement');
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
    background-color: #2D3545;
    color: #7ECAFA;
    padding: 4px 12px;
    border-radius: 12px;
    font-size: 14px;
    font-weight: 900;
}

.about-content {
    margin-top: 1px;
    display: flex;
    flex-direction: column;
    gap: 16px;
}
</style>