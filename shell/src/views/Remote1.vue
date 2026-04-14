<template>
  <div>
    <article v-if="isLoading" aria-busy="true">Conectando con Remote 1...</article>
    <article v-else-if="isError">Error al conectar con Remote 1</article>
    <template v-else>
      <p>Contador local del Shell: {{ shellCounter }}</p>
      <button @click="sendToRemote">Enviar contador al Remote: {{ shellCounter }}</button>
      <p>Contador recibido del Remote: {{ remoteCounter }}</p>
    </template>
    <hr />
    <RemoteApp
      :src="remoteUrl"
      title="Remote 1"
    />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { RemoteApp, useRemote } from '@sprlab/microfront/shell'

const remoteUrl = import.meta.env.VITE_REMOTE1_URL
const { sendMessage, onMessage, isLoading, isError } = useRemote()

const shellCounter = ref(0)
const remoteCounter = ref(0)

function sendToRemote() {
  shellCounter.value++
  sendMessage({ counter: shellCounter.value })
}

onMessage((payload: any, metadata) => {
  console.log(`Mensaje de: ${metadata.appName}`)
  remoteCounter.value = payload.counter
})
</script>
