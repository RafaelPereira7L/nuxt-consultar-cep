<template>
  <main class="content">
    <div v-if="error" class="alert alert-error shadow-lg text-white w-96">
      <div>
        <svg
xmlns="http://www.w3.org/2000/svg" class="stroke-current flex-shrink-0 h-6 w-6" fill="none"
          viewBox="0 0 24 24">
          <path
stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
            d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z" />
        </svg>
        <span>Erro! Não foi possivel encontrar o CEP informado.</span>
      </div>
    </div>
    <div class="box w-96 mockup-code shadow-sm">
      <section class="flex flex-col items-center justify-center gap-4">
        <input
          v-model="cep"
          type="text" placeholder="Informe o CEP" class="input input-bordered input-primary max-w-xs"
          @keyup.enter="getCep" />
        <button class="btn btn-primary" @click="getCep">Buscar</button>
      </section>
    </div>

    <div v-if="data && !error" class="card w-96 bg-neutral text-neutral-content">
      <div class="card-body items-center text-center">
        <h1 class="card-title font-bold text-2xl">Resultado 👇</h1>
        <section class="text-start">
          <p class="text-purple-600 font-bold">CEP: <span class="text-white">{{ data?.cep }}</span></p>
          <p class="text-purple-600 font-bold">Logradouro: <span class="text-white">{{ data?.logradouro }}</span></p>
          <p class="text-purple-600 font-bold">Complemento: <span class="text-white">{{ data?.complemento }}</span></p>
          <p class="text-purple-600 font-bold">Bairro: <span class="text-white">{{ data?.bairro }}</span></p>
          <p class="text-purple-600 font-bold">Localidade: <span class="text-white">{{ data?.localidade }}</span></p>
          <p class="text-purple-600 font-bold">UF: <span class="text-white">{{ data?.uf }}</span></p>
        </section>
      </div>
    </div>

  </main>
</template>

<script setup lang="ts">
import axios, { AxiosResponse } from 'axios';
import { ref } from 'vue'
const cep = ref<string>();
const error = ref(false);

interface Data {
  cep: string;
  logradouro: string;
  complemento: string;
  bairro: string;
  localidade: string;
  uf: string;
}
const data = ref<Data>();

const getCep = async () => {
  await axios.get(`https://viacep.com.br/ws/${cep.value}/json/`)
    .then((response: AxiosResponse<Data>) => {
      data.value = response.data;
      error.value = false;
      return data;
    })
    .catch(() => {
      error.value = true;
    });
}

</script>

<style>
.content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  gap: 2rem;
}

.box>section {
  height: 90%;
}
</style>
