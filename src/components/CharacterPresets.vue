<template>
    <div class="character-presets">
        <h3 class="presets-title">角色预设</h3>

        <div class="presets-list">
            <div
                v-for="preset in presets"
                :key="preset.name"
                class="preset-item"
                @click="$emit('apply-preset', preset.stats)"
            >
                <div class="preset-avatar">
                    {{ preset.element }}
                </div>
                <div class="preset-info">
                    <h4>{{ preset.name }}</h4>
                    <p>{{ preset.path }}</p>
                    <small>{{ preset.description }}</small>
                </div>
            </div>
        </div>

        <div class="quick-actions">
            <h4>快速设置</h4>
            <button
                v-for="template in quickTemplates"
                :key="template.name"
                class="btn btn-secondary template-btn"
                @click="$emit('apply-preset', template.stats)"
            >
                {{ template.name }}
            </button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'CharacterPresets',
    emits: ['apply-preset'],
    data() {
        return {
            presets: [
                {
                    name: '希儿',
                    element: '⚡',
                    path: '巡猎',
                    description: '量子属性单体输出',
                    stats: {
                        attack: 2400,
                        critRate: 65,
                        critDamage: 140,
                        skillMultiplier: 220,
                        damageBonus: 38.4,
                        defenseIgnore: 0
                    }
                },
                {
                    name: '景元',
                    element: '⚡',
                    path: '智识',
                    description: '雷属性群体输出',
                    stats: {
                        attack: 2200,
                        critRate: 55,
                        critDamage: 120,
                        skillMultiplier: 180,
                        damageBonus: 28.8,
                        defenseIgnore: 0
                    }
                },
                {
                    name: '刃',
                    element: '🌪️',
                    path: '毁灭',
                    description: '风属性生命转换输出',
                    stats: {
                        attack: 1800,
                        critRate: 70,
                        critDamage: 160,
                        skillMultiplier: 240,
                        damageBonus: 43.2,
                        defenseIgnore: 0
                    }
                },
                {
                    name: '卡芙卡',
                    element: '⚡',
                    path: '虚无',
                    description: '雷属性持续伤害',
                    stats: {
                        attack: 2100,
                        critRate: 45,
                        critDamage: 100,
                        skillMultiplier: 160,
                        damageBonus: 24,
                        defenseIgnore: 0
                    }
                },
                {
                    name: '符玄',
                    element: '⚡',
                    path: '巡猎',
                    description: '量子属性暴击流',
                    stats: {
                        attack: 2300,
                        critRate: 75,
                        critDamage: 180,
                        skillMultiplier: 200,
                        damageBonus: 33.6,
                        defenseIgnore: 15
                    }
                }
            ],
            quickTemplates: [
                {
                    name: '新手模板',
                    stats: {
                        attack: 1500,
                        critRate: 25,
                        critDamage: 80,
                        skillMultiplier: 100,
                        damageBonus: 0,
                        defenseIgnore: 0
                    }
                },
                {
                    name: '进阶模板',
                    stats: {
                        attack: 2000,
                        critRate: 50,
                        critDamage: 120,
                        skillMultiplier: 150,
                        damageBonus: 20,
                        defenseIgnore: 0
                    }
                },
                {
                    name: '毕业模板',
                    stats: {
                        attack: 2500,
                        critRate: 70,
                        critDamage: 160,
                        skillMultiplier: 220,
                        damageBonus: 40,
                        defenseIgnore: 20
                    }
                }
            ]
        }
    }
}
</script>

<style scoped>
.character-presets {
    padding: 25px;
}

.presets-title {
    color: #333;
    margin-bottom: 20px;
    font-size: 1.3em;
    border-bottom: 2px solid #667eea;
    padding-bottom: 8px;
}

.presets-list {
    margin-bottom: 30px;
}

.preset-item {
    display: flex;
    align-items: center;
    padding: 15px;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.3s ease;
    margin-bottom: 10px;
    border: 2px solid transparent;
}

.preset-item:hover {
    background: #f8f9fa;
    border-color: #667eea;
    transform: translateX(5px);
}

.preset-avatar {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: linear-gradient(135deg, #667eea, #764ba2);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.2em;
    margin-right: 15px;
}

.preset-info h4 {
    margin: 0 0 5px 0;
    color: #333;
    font-size: 1.1em;
}

.preset-info p {
    margin: 0 0 5px 0;
    color: #666;
    font-size: 0.9em;
}

.preset-info small {
    color: #888;
    font-size: 0.8em;
}

.quick-actions {
    border-top: 1px solid #eee;
    padding-top: 20px;
}

.quick-actions h4 {
    margin-bottom: 15px;
    color: #555;
}

.template-btn {
    width: 100%;
    margin-bottom: 8px;
    text-align: left;
    font-size: 0.9em;
}

.template-btn:last-child {
    margin-bottom: 0;
}
</style>
