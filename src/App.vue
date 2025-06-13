<template>
    <div id="app">
        <header class="app-header">
            <div class="container">
                <h1 class="app-title">
                    <span class="icon">⭐</span>
                    崩坏：星穹铁道 伤害计算器
                </h1>
                <p class="app-subtitle">精确计算角色技能期望伤害</p>
            </div>
        </header>

        <main class="app-main">
            <div class="container">
                <div class="calculator-layout">
                    <aside class="sidebar">
                        <CharacterPresets @apply-preset="applyPreset" />
                    </aside>

                    <section class="main-content">
                        <DamageCalculator ref="calculator" />
                    </section>
                </div>
            </div>
        </main>

        <footer class="app-footer">
            <div class="container">
                <p>&copy; 2025 星穹铁道伤害计算器 | Made with ❤️ by 芝西 董</p>
            </div>
        </footer>
    </div>
</template>

<script>
import DamageCalculator from './components/DamageCalculator.vue'
import CharacterPresets from './components/CharacterPresets.vue'

export default {
    name: 'App',
    components: {
        DamageCalculator,
        CharacterPresets
    },
    methods: {
        applyPreset(preset) {
            this.$refs.calculator.applyPreset(preset)
        }
    }
}
</script>

<style scoped>
.app-header {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 40px 0;
    text-align: center;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
}

.app-title {
    font-size: 2.5em;
    margin-bottom: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 15px;
}

.icon {
    font-size: 1.2em;
    animation: pulse 2s infinite;
}

@keyframes pulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.1); }
}

.app-subtitle {
    font-size: 1.2em;
    opacity: 0.9;
}

.app-main {
    min-height: calc(100vh - 200px);
    padding: 40px 0;
}

.calculator-layout {
    display: grid;
    grid-template-columns: 300px 1fr;
    gap: 30px;
}

.sidebar {
    background: white;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    height: fit-content;
    position: sticky;
    top: 20px;
}

.main-content {
    background: white;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.app-footer {
    background: #2c3e50;
    color: white;
    text-align: center;
    padding: 20px 0;
}

@media (max-width: 768px) {
    .app-title {
        font-size: 2em;
        flex-direction: column;
        gap: 10px;
    }

    .calculator-layout {
        grid-template-columns: 1fr;
        gap: 20px;
    }

    .sidebar {
        position: static;
        order: 2;
    }
}
</style>
