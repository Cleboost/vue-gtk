<script setup lang="ts">
import { Switch, Button, BoxRow, RowButton, RowLink, About } from "vue-gtk";
import { ref } from "vue";

const showAbout = ref(false);

const creditData = [
    {
        name: "Code par",
        content: [
            {
                type: "link",
                texte: "Cleboost",
                link: "https://github.com/Cleboost"
            }
        ]
    },
    {
        name: "Design de",
        content: [
            {
                type: "text",
                texte: "Interface inspirée de GTK"
            }
        ]
    },
    {
        name: "Icônes par",
        content: [
            {
                type: "link",
                texte: "Tabler Icons",
                link: "https://tabler-icons.io"
            }
        ]
    }
];

const openAbout = () => {
    showAbout.value = true;
};

const closeAbout = () => {
    showAbout.value = false;
};

const handleDepannage = () => {
    console.log('Dépannage clicked');
    // Navigation logic here
};

const handleCredit = () => {
    console.log('Credits clicked');
    // Navigation logic here
};

const handleLegal = () => {
    console.log('Legal clicked');
    // Navigation logic here
};

const handleRemerciement = () => {
    console.log('Remerciement clicked');
    // Navigation logic here
};
</script>

<template>
    <main>
        <div class="demo-container">
            <div class="header-section">
                <h1>Vue GTK Components</h1>
                <Button @click="openAbout">About</Button>
            </div>
            
            <section class="component-section">
                <h2>Switch</h2>
                <div class="component-demo">
                    <div class="switch-row">
                        <Switch size="small" />
                        <Switch size="medium" />
                        <Switch size="large" />
                    </div>
                </div>
            </section>

            <section class="component-section">
                <h2>Button</h2>
                <div class="component-demo">
                    <div class="button-row">
                        <Button size="small">Small Button</Button>
                        <Button size="medium">Medium Button</Button>
                        <Button size="large">Large Button</Button>
                    </div>
                    
                    <div class="button-row">
                        <Button variant="primary">Primary Button</Button>
                        <Button variant="secondary">Secondary Button</Button>
                        <Button variant="outline">Outline Button</Button>
                    </div>
                    
                    <div class="button-row">
                        <Button>Ajouter des fichiers</Button>
                    </div>
                </div>
            </section>

            <section class="component-section">
                <h2>Row Components</h2>
                <div class="component-demo">
                    <BoxRow>
                        <RowButton text="Credits" subtitle="About the developers" />
                        <RowLink text="GitHub Repository" href="https://github.com/Cleboost/vue-gtk" subtitle="Source code on GitHub" />
                        <RowLink text="Documentation" href="https://vuejs.org" subtitle="Official Vue.js docs" />
                        <RowButton text="Legal" :is-last="true" />
                    </BoxRow>
                </div>
            </section>
        </div>
        
        <!-- About Modal -->
        <About 
            :is-visible="showAbout"
            nom="Vue GTK"
            auteur="Cleboost"
            version="0.1.0"
            web="https://github.com/Cleboost/vue-gtk"
            issue="https://github.com/Cleboost/vue-gtk/issues"
            depannage="Troubleshooting"
            :credit="creditData"
            mentionLegales="Legal"
            remerciement="Thanks"
            :show-close-button="true"
            @close="closeAbout"
            @depannage="handleDepannage"
            @credit="handleCredit"
            @mentionLegales="handleLegal"
            @remerciement="handleRemerciement"
        />
    </main>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Cantarell:ital,wght@0,400;0,700;1,400;1,700&display=swap');

body {
    background-color: #272830;
    color: white;
    font-family: 'Cantarell', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    margin: 0;
    padding: 0;
}

.demo-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 40px 20px;
}

.header-section {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 40px;
}

h1 {
    margin: 0;
    color: #fff;
    font-size: 2.5rem;
    font-weight: 600;
}

.component-section {
    margin-bottom: 40px;
}

.component-section h2 {
    color: #fff;
    margin-bottom: 20px;
    font-size: 1.5rem;
    font-weight: 500;
}

.component-demo {
    display: flex;
    flex-direction: column;
    gap: 16px;
    margin-bottom: 20px;
    padding: 20px;
    background-color: rgba(255, 255, 255, 0.05);
    border-radius: 12px;
    border: 1px solid rgba(255, 255, 255, 0.1);
}

.switch-row {
    display: flex;
    gap: 16px;
    align-items: center;
    flex-wrap: wrap;
}

.button-row {
    display: flex;
    gap: 16px;
    align-items: center;
    flex-wrap: wrap;
}
</style>
