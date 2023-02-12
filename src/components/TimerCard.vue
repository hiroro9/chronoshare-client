<template>

  <VCard :elevation="2" max-width="600">
    <VCardTitle>
      Timer id: {{ id }}
    </VCardTitle>
    <VContainer>
      <VRow>
        <VCol>
          <h2>
            {{ Math.floor(remain / 60) + ":" + remain % 60 }}
          </h2>
        </VCol>
      </VRow>
      <VRow>
        <VCol>
          <VBtn @click="readTimer">get</VBtn>
        </VCol>
        <VCol>
          <VBtn @click="startTimer">start</VBtn>
        </VCol>
        <VCol>
          <VBtn @click="restartTimer">restart</VBtn>
        </VCol>
        <VCol>
          <VBtn @click="resetTimer">reset</VBtn>
        </VCol>
        <VCol>
          <VBtn @click="stopTimer">stop</VBtn>
        </VCol>
      </VRow>

    </VContainer>
  </VCard>
</template>

<script lang="ts" setup>

import { ref, onMounted } from "vue"

const remain = ref(0)
const host = import.meta.env.VITE_HOST
const wshost = import.meta.env.VITE_WSHOST


const props = defineProps({
  id: String
})

const socket = new WebSocket(`${wshost}/timer/${props.id}`)

const initTimer = () => {
  console.log(process.env.TEST)
  fetch(`${host}/init/${props.id}`)
}

onMounted(() => {
  initTimer()

  socket.onmessage = (event) => {
    console.log(remain.value)
    remain.value = event.data
  }
})


const readTimer = () => {
  fetch(`${host}/read/${props.id}`)
    .then((res) => res.json())
    .then((data) => {
      remain.value = data
    })
}

const startTimer = () => {
  fetch(`${host}/start/${props.id}`)
}

const restartTimer = () => {
  fetch(`${host}/restart/${props.id}`)
}

const resetTimer = () => {
  fetch(`${host}/reset/${props.id}`)
}

const stopTimer = () => {
  fetch(`${host}/stop/${props.id}`)
}
</script>
