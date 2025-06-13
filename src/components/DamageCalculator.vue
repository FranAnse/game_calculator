<template>
    <div class="damage-calculator">
        <div class="calculator-content">
            <div class="input-sections">
                <!-- 基础属性 -->
                <div class="card">
                    <h3 class="section-title">基础属性</h3>
                    <div class="grid grid-2">
                        <div class="form-group">
                            <label class="form-label" for="attack">攻击力</label>
                            <input
                                id="attack"
                                v-model.number="stats.attack"
                                type="number"
                                class="form-control"
                                placeholder="输入攻击力"
                                min="0"
                            />
                        </div>

                        <div class="form-group">
                            <label class="form-label" for="critRate">暴击率 (%)</label>
                            <input
                                id="critRate"
                                v-model.number="stats.critRate"
                                type="number"
                                class="form-control"
                                placeholder="输入暴击率"
                                min="0"
                                max="100"
                                step="0.1"
                            />
                        </div>

                        <div class="form-group">
                            <label class="form-label" for="critDamage">暴击伤害 (%)</label>
                            <input
                                id="critDamage"
                                v-model.number="stats.critDamage"
                                type="number"
                                class="form-control"
                                placeholder="输入暴击伤害"
                                min="0"
                                step="0.1"
                            />
                        </div>

                        <div class="form-group">
                            <label class="form-label" for="skillMultiplier">技能倍率 (%)</label>
                            <input
                                id="skillMultiplier"
                                v-model.number="stats.skillMultiplier"
                                type="number"
                                class="form-control"
                                placeholder="输入技能倍率"
                                min="0"
                                step="0.1"
                            />
                        </div>
                    </div>
                </div>

                <!-- 额外属性 -->
                <div class="card">
                    <h3 class="section-title">额外属性</h3>
                    <div class="grid grid-2">
                        <div class="form-group">
                            <label class="form-label" for="damageBonus">伤害加成 (%)</label>
                            <input
                                id="damageBonus"
                                v-model.number="stats.damageBonus"
                                type="number"
                                class="form-control"
                                placeholder="属性伤害加成"
                                min="0"
                                step="0.1"
                            />
                        </div>

                        <div class="form-group">
                            <label class="form-label" for="defenseIgnore">无视防御 (%)</label>
                            <input
                                id="defenseIgnore"
                                v-model.number="stats.defenseIgnore"
                                type="number"
                                class="form-control"
                                placeholder="无视防御百分比"
                                min="0"
                                max="100"
                                step="0.1"
                            />
                        </div>

                        <div class="form-group">
                            <label class="form-label" for="enemyDefense">敌人防御力</label>
                            <input
                                id="enemyDefense"
                                v-model.number="enemy.defense"
                                type="number"
                                class="form-control"
                                placeholder="敌人防御力"
                                min="0"
                            />
                        </div>

                        <div class="form-group">
                            <label class="form-label" for="characterLevel">角色等级</label>
                            <input
                                id="characterLevel"
                                v-model.number="character.level"
                                type="number"
                                class="form-control"
                                placeholder="角色等级"
                                min="1"
                                max="80"
                            />
                        </div>
                    </div>
                </div>

                <!-- 计算按钮 -->
                <div class="card">
                    <div class="calculation-controls">
                        <button @click="calculateDamage" class="btn btn-primary calculate-btn">
                            🧮 计算伤害期望
                        </button>
                        <button @click="resetForm" class="btn btn-secondary">
                            🔄 重置数据
                        </button>
                    </div>
                </div>
            </div>

            <!-- 结果显示 -->
            <div v-if="result.calculated" class="result-section">
                <div class="card result-card">
                    <h3 class="section-title">💥 计算结果</h3>

                    <div class="result-summary">
                        <div class="damage-display">
                            <span class="damage-label">期望伤害</span>
                            <span class="damage-value">{{ formatNumber(result.expectedDamage) }}</span>
                        </div>
                    </div>

                    <div class="result-details">
                        <div class="grid grid-3">
                            <div class="result-item">
                                <div class="result-label">基础伤害</div>
                                <div class="result-value">{{ formatNumber(result.baseDamage) }}</div>
                            </div>
                            <div class="result-item">
                                <div class="result-label">非暴击伤害</div>
                                <div class="result-value">{{ formatNumber(result.nonCritDamage) }}</div>
                            </div>
                            <div class="result-item">
                                <div class="result-label">暴击伤害</div>
                                <div class="result-value">{{ formatNumber(result.critDamage) }}</div>
                            </div>
                        </div>
                    </div>

                    <div class="damage-breakdown">
                        <h4>📊 伤害构成分析</h4>
                        <div class="breakdown-content">
                            <div class="breakdown-item">
                                <span>暴击概率: </span>
                                <strong>{{ (stats.critRate || 0).toFixed(1) }}%</strong>
                            </div>
                            <div class="breakdown-item">
                                <span>非暴击概率: </span>
                                <strong>{{ (100 - (stats.critRate || 0)).toFixed(1) }}%</strong>
                            </div>
                            <div class="breakdown-formula">
                                <span>期望计算公式:</span>
                                <div class="formula">
                                    {{ formatNumber(result.nonCritDamage) }} × {{ (100 - (stats.critRate || 0)).toFixed(1) }}% +
                                    {{ formatNumber(result.critDamage) }} × {{ (stats.critRate || 0).toFixed(1) }}% =
                                    <strong>{{ formatNumber(result.expectedDamage) }}</strong>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'DamageCalculator',
    data() {
        return {
            stats: {
                attack: 2000,
                critRate: 50,
                critDamage: 100,
                skillMultiplier: 100,
                damageBonus: 0,
                defenseIgnore: 0
            },
            enemy: {
                defense: 500
            },
            character: {
                level: 70
            },
            result: {
                calculated: false,
                baseDamage: 0,
                nonCritDamage: 0,
                critDamage: 0,
                expectedDamage: 0
            }
        }
    },
    methods: {
        calculateDamage() {
            if (!this.validateInputs()) {
                alert('请检查输入值是否正确！\\n- 所有数值必须大于等于0\\n- 暴击率不能超过100%')
                return
            }

            // 基础伤害计算
            const baseDamage = this.stats.attack * (this.stats.skillMultiplier / 100)

            // 伤害加成计算
            const damageMultiplier = 1 + (this.stats.damageBonus / 100)

            // 防御减伤计算
            const effectiveDefense = this.enemy.defense * (1 - (this.stats.defenseIgnore / 100))
            const defenseMultiplier = (this.character.level * 10 + 200) /
                (this.character.level * 10 + 200 + effectiveDefense)

            // 最终基础伤害
            const finalBaseDamage = baseDamage * damageMultiplier * defenseMultiplier

            // 非暴击伤害
            const nonCritDamage = finalBaseDamage

            // 暴击伤害
            const critDamage = finalBaseDamage * (1 + (this.stats.critDamage / 100))

            // 期望伤害计算
            const critRate = Math.min(this.stats.critRate / 100, 1)
            const expectedDamage = nonCritDamage * (1 - critRate) + critDamage * critRate

            // 更新结果
            this.result = {
                calculated: true,
                baseDamage: Math.round(baseDamage),
                nonCritDamage: Math.round(nonCritDamage),
                critDamage: Math.round(critDamage),
                expectedDamage: Math.round(expectedDamage)
            }
        },

        validateInputs() {
            const requiredFields = ['attack', 'critRate', 'critDamage', 'skillMultiplier']
            for (let field of requiredFields) {
                if (!this.stats[field] || this.stats[field] < 0) {
                    return false
                }
            }

            if (this.stats.critRate > 100) {
                return false
            }

            return true
        },

        resetForm() {
            this.stats = {
                attack: 2000,
                critRate: 50,
                critDamage: 100,
                skillMultiplier: 100,
                damageBonus: 0,
                defenseIgnore: 0
            }
            this.enemy = { defense: 500 }
            this.character = { level: 70 }
            this.result = {
                calculated: false,
                baseDamage: 0,
                nonCritDamage: 0,
                critDamage: 0,
                expectedDamage: 0
            }
        },

        applyPreset(preset) {
            this.stats = { ...preset }
            this.result.calculated = false
        },

        formatNumber(num) {
            return num.toLocaleString()
        }
    }
}
</script>

<style scoped>
.damage-calculator {
    padding: 30px;
}

.section-title {
    color: #333;
    margin-bottom: 20px;
    font-size: 1.3em;
    display: flex;
    align-items: center;
    gap: 8px;
}

.calculation-controls {
    display: flex;
    gap: 15px;
    justify-content: center;
}

.calculate-btn {
    font-size: 1.1em;
    padding: 15px 30px;
}

.result-section {
    margin-top: 30px;
}

.result-card {
    background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
    border: 2px solid #667eea;
}

.result-summary {
    text-align: center;
    margin-bottom: 25px;
}

.damage-display {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 10px;
}

.damage-label {
    font-size: 1.2em;
    color: #666;
    font-weight: 500;
}

.damage-value {
    font-size: 3em;
    font-weight: 700;
    background: linear-gradient(135deg, #667eea, #764ba2);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
}

.result-details {
    margin-bottom: 25px;
}

.result-item {
    text-align: center;
    padding: 15px;
    background: white;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.result-label {
    font-size: 0.9em;
    color: #666;
    margin-bottom: 5px;
}

.result-value {
    font-size: 1.3em;
    font-weight: 600;
    color: #333;
}

.damage-breakdown {
    background: white;
    border-radius: 8px;
    padding: 20px;
}

.damage-breakdown h4 {
    margin-bottom: 15px;
    color: #333;
}

.breakdown-content {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.breakdown-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.breakdown-formula {
    margin-top: 15px;
    padding-top: 15px;
    border-top: 1px solid #eee;
}

.formula {
    margin-top: 8px;
    padding: 10px;
    background: #f8f9fa;
    border-radius: 4px;
    font-family: 'Courier New', monospace;
    font-size: 0.9em;
    word-break: break-all;
}

@media (max-width: 768px) {
    .damage-calculator {
        padding: 20px;
    }

    .calculation-controls {
        flex-direction: column;
    }

    .damage-value {
        font-size: 2.5em;
    }

    .breakdown-item {
        flex-direction: column;
        align-items: flex-start;
        gap: 5px;
    }
}
</style>
