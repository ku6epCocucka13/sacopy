<template>
  <div class="subnet-calculator">
    <div class="calculator-card">
      <h2 class="calculator-title">Калькулятор подсетей</h2>
      
      <div class="input-group">
        <label class="input-label" for="ip-address">IP адрес:</label>
        <input
          id="ip-address"
          v-model="ipAddress"
          type="text"
          placeholder="192.168.1.150"
          class="ip-input"
          :class="{ 'input-error': !isIpValid }"
          @keyup.enter="calculate"
        />
        <div v-if="!isIpValid && ipAddress" class="error-message">
          Неверный формат IP адреса
        </div>
      </div>

      <div class="input-group">
        <label class="input-label" for="subnet-mask">Сетевая маска:</label>
        <select
          id="subnet-mask"
          v-model="selectedMask"
          class="mask-select"
        >
          <option
            v-for="option in SUBNET_MASK_OPTIONS"
            :key="option.value"
            :value="option.value"
          >
            {{ option.label }}
          </option>
        </select>
      </div>

      <button
        class="calculate-button"
        :disabled="!canCalculate"
        @click="calculate"
      >
        Рассчитать
      </button>

      <div v-if="showResults" class="results-section">
        // eslint-disable-next-line prettier/prettier
        <h3 class="results-title">Результаты расчета:</h3>
        
        <div class="result-item">
          <span class="result-label">IP адрес:</span>
          <span class="result-value">{{ results.ip }}</span>
        </div>
        
        <div class="result-item">
          <span class="result-label">Маска подсети:</span>
          <span class="result-value">{{ results.mask }}</span>
        </div>
        
        <div class="result-item">
          <span class="result-label">Адрес сети:</span>
          <span class="result-value highlight">{{ results.networkAddress }}</span>
        </div>
        
        <div class="result-item">
          <span class="result-label">Количество адресов:</span>
          <span class="result-value highlight">{{ results.addressesCount }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, reactive } from 'vue'
import { SUBNET_MASK_OPTIONS } from '../constants/subnetMasks'
import { isIpValid, getNetworkAddress, getAddressesCount } from '../utils/ipCalculator'

const ipAddress = ref('192.168.1.150')
const selectedMask = ref('255.255.255.0')
const showResults = ref(false)

const results = reactive({
  ip: '',
  mask: '',
  networkAddress: '',
  addressesCount: 0
})

const isIpValid = computed(() => {
  return ipAddress.value ? isIpValid(ipAddress.value) : false
})

const canCalculate = computed(() => {
  return isIpValid.value && selectedMask.value
})

function calculate() {
  if (!canCalculate.value) return

  results.ip = ipAddress.value
  results.mask = selectedMask.value
  results.networkAddress = getNetworkAddress(ipAddress.value, selectedMask.value)
  results.addressesCount = getAddressesCount(selectedMask.value)
  
  showResults.value = true
}
</script>

<style scoped>
.subnet-calculator {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
}

.calculator-card {
  background: var(--card-bg);
  border-radius: 12px;
  padding: 24px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  border: 1px solid var(--border-color);
}

.calculator-title {
  color: var(--text-primary);
  margin-bottom: 24px;
  text-align: center;
  font-size: 1.5rem;
  font-weight: 600;
}

.input-group {
  margin-bottom: 20px;
}

.input-label {
  display: block;
  margin-bottom: 8px;
  color: var(--text-secondary);
  font-weight: 500;
}

.ip-input,
.mask-select {
  width: 100%;
  padding: 12px;
  border: 2px solid var(--border-color);
  border-radius: 8px;
  background: var(--input-bg);
  color: var(--text-primary);
  font-size: 14px;
  transition: border-color 0.3s ease;
}

.ip-input:focus,
.mask-select:focus {
  outline: none;
  border-color: var(--primary-color);
}

.ip-input.input-error {
  border-color: var(--error-color);
}

.error-message {
  color: var(--error-color);
  font-size: 12px;
  margin-top: 4px;
}

.calculate-button {
  width: 100%;
  padding: 12px;
  background: var(--primary-color);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.calculate-button:hover:not(:disabled) {
  background: var(--primary-hover);
}

.calculate-button:disabled {
  background: var(--disabled-color);
  cursor: not-allowed;
}

.results-section {
  margin-top: 24px;
  padding-top: 20px;
  border-top: 1px solid var(--border-color);
}

.results-title {
  color: var(--text-primary);
  margin-bottom: 16px;
  font-size: 1.2rem;
}

.result-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 0;
  border-bottom: 1px solid var(--border-light);
}

.result-item:last-child {
  border-bottom: none;
}

.result-label {
  color: var(--text-secondary);
  font-weight: 500;
}

.result-value {
  color: var(--text-primary);
  font-weight: 600;
}

.result-value.highlight {
  color: var(--primary-color);
}
</style>