<script setup lang="ts">
import { Switch, Button, BoxRow, RowButton, RowLink, About } from "vue-gtk";
import { Icon } from '@iconify/vue';
import { ref } from "vue";

const showAbout = ref(false);

const creditData = [
    {
        name: "Code par",
        content: [
            {
                type: "link" as const,
                texte: "Cleboost",
                link: "https://github.com/Cleboost"
            }
        ]
    },
    {
        name: "Design de",
        content: [
            {
                type: "text" as const,
                texte: "Interface inspirée de GTK"
            }
        ]
    },
    {
        name: "Icônes par",
        content: [
            {
                type: "link" as const,
                texte: "Tabler Icons",
                link: "https://tabler-icons.io"
            }
        ]
    }
];

const remerciementData = [
    {
        name: "Vue.js",
        quote: "Framework JavaScript progressif"
    },
    {
        name: "Tabler Icons",
        quote: "Icônes open source"
    },
    {
        name: "GTK",
        quote: "Toolkit pour interfaces graphiques"
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

const closeApp = () => {
    console.log('App closed');
    // In a real app, this would close the window
    alert('Application fermée !');
};
</script>

<template>
    <main>
        <div class="app-window">
            <div class="window-header">
                <div class="window-title">Vue GTK Demo</div>
                <div class="window-controls">
                    <div class="window-dots">
                        <div class="dot"></div>
                        <div class="dot"></div>
                        <div class="dot"></div>
                    </div>
                    <button class="window-close-button" @click="closeApp">
                        <Icon icon="tabler:x" />
                    </button>
                </div>
            </div>
            
            <div class="window-content">
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
            </div>
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
            :remerciement="remerciementData"
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
    background-color: #0a0a0a;
    color: white;
    font-family: 'Cantarell', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    margin: 0;
    padding: 20px;
    min-height: 100vh;
    box-sizing: border-box;
}

.app-window {
    background: rgba(18, 19, 29, 0.4);
    backdrop-filter: blur(30px);
    -webkit-backdrop-filter: blur(30px);
    border: 4px solid rgba(255, 255, 255, 0.05);
    border-radius: 20px;
    box-shadow: 
        0 12px 40px rgba(0, 0, 0, 0.2),
        inset 0 2px 0 rgba(255, 255, 255, 0.05),
        inset 0 -2px 0 rgba(0, 0, 0, 0.1);
    width: 100%;
    max-height: calc(100vh - 40px);
    display: flex;
    flex-direction: column;
    overflow: hidden;
}

.window-header {
    background: rgba(26, 27, 38, 0.3);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    border-bottom: 2px solid rgba(255, 255, 255, 0.05);
    padding: 16px 24px;
    display: flex;
    justify-content: center;
    align-items: center;
    user-select: none;
    position: relative;
}

.window-title {
    color: #ffffff;
    font-size: 14px;
    font-weight: 700;
}

.window-controls {
    position: absolute;
    right: 24px;
    top: 50%;
    transform: translateY(-50%);
    display: flex;
    align-items: center;
    gap: 16px;
}

.window-dots {
    display: flex;
    flex-direction: column;
    gap: 2px;
    cursor: pointer;
    padding: 8px;
    margin: -8px;
}

.dot {
    width: 3px;
    height: 3px;
    background-color: rgba(255, 255, 255, 0.6);
    border-radius: 50%;
}

.window-close-button {
    background: rgba(26, 27, 38, 0.4);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    border: 2px solid rgba(255, 255, 255, 0.05);
    border-radius: 50%;
    width: 28px;
    height: 28px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: all 0.3s ease;
    box-shadow: 
        0 6px 16px rgba(0, 0, 0, 0.15),
        inset 0 2px 0 rgba(255, 255, 255, 0.05),
        inset 0 -2px 0 rgba(0, 0, 0, 0.1);
}

.window-close-button:hover {
    background: rgba(42, 42, 58, 0.6);
    border-color: rgba(255, 255, 255, 0.1);
    box-shadow: 
        0 8px 24px rgba(0, 0, 0, 0.2),
        inset 0 2px 0 rgba(255, 255, 255, 0.1),
        inset 0 -2px 0 rgba(0, 0, 0, 0.15);
    transform: translateY(-2px);
}

.window-close-button::before,
.window-close-button::after {
    content: '';
    position: absolute;
    background-color: #666;
    width: 2px;
    height: 8px;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    image-rendering: pixelated;
    image-rendering: -moz-crisp-edges;
    image-rendering: crisp-edges;
}

.window-close-button::before {
    transform: translate(-50%, -50%) rotate(45deg);
}

.window-close-button::after {
    transform: translate(-50%, -50%) rotate(-45deg);
}

.window-content {
    flex: 1;
    overflow-y: auto;
    padding: 0 0 20px 0;
    min-height: 0;
}

.demo-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 40px 20px 60px 20px;
    background-color: transparent;
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
