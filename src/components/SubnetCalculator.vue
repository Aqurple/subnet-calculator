<script setup lang="ts">
import { ref, computed } from 'vue';
import { UiButton, UiInput, UiSelect, UiField } from 'aqurple-component-library';

// import 'aqurple-component-library/dist/UiButton/UiButton.css';
// import 'aqurple-component-library/dist/UiInput/UiInput.css';
// import 'aqurple-component-library/dist/UiSelect/UiSelect.css';
// import 'aqurple-component-library/dist/UiField/UiField.css';

import { maskOptions } from '../constants/maskOptions.ts';
import { isIpValid } from '../utils/isIpValid.ts';
import { getNetworkAddress } from '../utils/getNetworkAddress.ts';
import { getAddressesCount } from '../utils/getAddressesCount.ts';

const ipInput = ref('192.168.1.150');
const selectedMask = ref(maskOptions[8].value);

const result = ref<{
  ip: string;
  mask: string;
  network: string;
  count: number;
} | null>(null);

const isValid = computed(() => isIpValid(ipInput.value));

const selectOptions = computed(() => maskOptions.map(opt => opt.value));

const handleCalculate = () => {
  if (!isValid.value) return;

  result.value = {
    ip: ipInput.value,
    mask: selectedMask.value,
    network: getNetworkAddress(ipInput.value, selectedMask.value),
    count: getAddressesCount(selectedMask.value),
  };
};
</script>

<template>
  <div class="calculator">
    <h2 class="calculator__title">Subnet Calculator</h2>
    
    <div class="calculator__form">
      <UiField label="IP address">
        <UiInput 
          v-model="ipInput" 
          placeholder="example: 192.168.1.1"
          @keydown.enter="handleCalculate"
        />
        <span v-if="ipInput.length > 0 && !isValid" class="error-msg">
          invalid IP address
        </span>
      </UiField>

      <UiField label="Network mask">
        <UiSelect 
          v-model="selectedMask" 
          :options="selectOptions" 
        />
      </UiField>

      <UiButton 
        layout="primary" 
        :isDisabled="!isValid" 
        @click="handleCalculate"
      >
        CALCULATE
      </UiButton>
    </div>

    <div v-if="result" class="calculator__results">
      <div class="result-item">
        <span class="result-label">IP address / netmask:</span>
        <span class="result-value">{{ result.ip }} / {{ result.mask }}</span>
      </div>
      <div class="result-item">
        <span class="result-label">network address: </span>
        <span class="result-value">{{ result.network }}</span>
      </div>
      <div class="result-item">
        <span class="result-label">count of hosts:</span>
        <span class="result-value">{{ result.count }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.calculator {
  background-color: var(--color-white);
  border: 1px solid var(--color-gray);
  border-radius: 20px;
  padding: 2.5rem;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  
  &__title {
    text-align: center;
    color: var(--color-black);
    margin-bottom: 2rem;
  }

  &__form {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
  }

  .error-msg {
    margin-top: 4px;
    font-size: 0.8rem;
    color: var(--color-error);
  }

  &__results {
    margin-top: 2rem;
    padding-top: 1.5rem;
    border-top: 1px solid var(--color-gray);
    display: flex;
    flex-direction: column;
    gap: 1rem;

    .result-item {
      display: flex;
      justify-content: space-between;
      align-items: center;

      .result-label {
        color: var(--color-gray-dark);
      }
      
      .result-value {
        font-weight: 700;
        color: var(--color-primary);
      }
    }
  }
}
</style>