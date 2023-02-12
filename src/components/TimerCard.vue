<template>

  <VCard :elevation="2" max-width="600">
    <VCardTitle>
      Timer id: {{ id }}
    </VCardTitle>
    <VContainer>
      <VRow>
        <VCol>
          {{ Math.floor(remain / 60) + ":" + remain % 60 }}
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
const host = "mb-commissions-maintaining-longer.trycloudflare.com"
// const host = "localhost:8080"


const props = defineProps({
  id: String
})

const socket = new WebSocket(`ws://${host}/timer/${props.id}`)




const initTimer = () => {
  fetch(`http://${host}/init/${props.id}`)
}

onMounted(() => {
  initTimer()

  socket.onmessage = (event) => {
    console.log(remain.value)
    remain.value = event.data
  }
})


const readTimer = () => {
  fetch(`http://${host}/read/${props.id}`)
    .then((res) => res.json())
    .then((data) => {
      remain.value = data
    })
}

const startTimer = () => {
  fetch(`http://${host}/start/${props.id}`)
}

const restartTimer = () => {
  fetch(`http://${host}/restart/${props.id}`)
}

const resetTimer = () => {
  fetch(`http://${host}/reset/${props.id}`)
}

const stopTimer = () => {
  fetch(`http://${host}/stop/${props.id}`)
}
</script>
