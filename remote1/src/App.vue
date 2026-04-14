<template>
  <h1>Remote 1</h1>
  <p>Contador del Shell: {{ shellCounter }}</p>
  <button @click="sendToShell">
    Enviar contador al Shell: {{ localCounter }}
  </button>
  <br /><br />
  <button @click="showLorem = !showLorem">
    {{ showLorem ? 'Ocultar' : 'Mostrar' }} Lorem Ipsum
  </button>
  <LoremIpsum v-if="showLorem" />
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { send, onMessage } from '@sprlab/microfront/remote'
import LoremIpsum from '@/components/LoremIpsum.vue'

const shellCounter = ref(0)
const localCounter = ref(0)
const showLorem = ref(false)

onMessage((payload: any) => {
  shellCounter.value = payload.counter
})

function sendToShell() {
  localCounter.value++
  send({ counter: localCounter.value })
}
</script>
