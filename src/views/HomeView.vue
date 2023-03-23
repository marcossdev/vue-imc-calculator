<script setup lang="ts">
import FormField from '@/components/FormField.vue';
import { ref } from 'vue';

const _height = ref<number>();
const _weight = ref<number>();
const _imc = ref<number>(0);
const _resultData = ref<{ color?: string; classification?: string }>();
const _classification = ref<string>('?');

const COLORS = [
  [(imc: number) => imc < 18.5, 'text-yellow-300', 'Abaixo do peso'],
  [(imc: number) => imc < 25, 'text-green-500', 'Normal'],
  [(imc: number) => imc < 30, 'text-orange-500', 'Acima do peso'],
  [(imc: number) => imc < 35, 'text-red-500', 'Obesidade I'],
  [(imc: number) => imc < 40, 'text-red-500', 'Obesidade II'],
  [(imc: number) => imc > 40, 'text-red-500', 'Obesidade III'],
] as const;

const getColor = () => {
  const data = COLORS.find(([imc]) => imc(_imc.value));
  return { color: data?.[1], classification: data?.[2] };
};

const calculateIMC = () => {
  if (!_height.value || !_weight.value) {
    return;
  }

  const height = parseFloat(_height.value.toString().replace(',', '.'));
  const weight = parseFloat(_weight.value.toString().replace(',', '.'));

  const imc = Number((weight / height ** 2).toFixed(1));

  _imc.value = imc;

  const classificatin = getColor();
  _resultData.value = classificatin;
  _classification.value = `${_imc.value} - ${_resultData.value?.classification}`;
};
</script>

<template>
  <main
    class="flex min-h-screen flex-col items-center justify-center bg-zinc-900 font-mavan text-white"
  >
    <div>
      <h2 class="mb-4 text-center text-3xl font-bold md:text-left">Calculadora de IMC</h2>
      <form @submit.prevent="calculateIMC" class="flex w-[340px] flex-col md:w-[480px]">
        <FormField label="Altura (m)">
          <input
            type="text"
            class="apare h-[48px] w-full rounded-md border-[1px] border-zinc-800 bg-transparent px-4 outline-none hover:border-blue-600"
            v-model="_height"
            required
            placeholder="Exemplo: 1.75"
            max="1000"
          />
        </FormField>
        <FormField label="Peso (kg)">
          <input
            type="number"
            class="h-[48px] w-full rounded-md border-[1px] border-zinc-800 bg-transparent px-4 outline-none hover:border-blue-600"
            v-model="_weight"
            required
            placeholder="Exemplo: 72"
            max="1000"
          />
        </FormField>
        <div class="flex w-full flex-row justify-between">
          <button
            type="submit"
            class="md:[w-36] h-[48px] w-[120px] rounded-lg border-[1px] border-zinc-800 bg-transparent text-center hover:border-blue-600 active:scale-[0.98]"
          >
            Calcular
          </button>
          <div
            class="flex h-[48px] w-[60%] items-center justify-center rounded-lg border-[1px] border-zinc-800 bg-transparent md:w-[60%]"
          >
            <p>
              IMC:
              <span :class="_resultData?.color">{{ _classification }}</span>
            </p>
          </div>
        </div>
      </form>
    </div>
    <div class="absolute bottom-5">
      <a href="https://github.com/marcossdev/" target="_blank"
        ><img src="../assets/images/github-mark-white.png" alt="GitHub" width="30" height="30"
      /></a>
    </div>
  </main>
</template>

<style>
input[type='number']::-webkit-inner-spin-button {
  -webkit-appearance: none;
}
input[type='number'] {
  -moz-appearance: textfield;
  appearance: textfield;
}
</style>
