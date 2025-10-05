<script setup lang="ts">
import { Switch, Button } from "vue-gtk";
</script>

<template>
    <main>
        <div class="demo-container">
            <h1>Vue GTK Components</h1>
            
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
        </div>
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

h1 {
    text-align: center;
    margin-bottom: 40px;
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
