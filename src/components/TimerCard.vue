<template>
  <h1> Timer id: {{ id }}</h1>

  <VCard>
    <p>
      {{ remain }}
    </p>
    <p>
      <VBtn @click="readTimer">get</VBtn>
    </p>
    <p>
      <VBtn @click="startTimer">start</VBtn>
    </p>
    <p>
      <VBtn @click="restartTimer">restart</VBtn>
    </p>
    <p>
      <VBtn @click="resetTimer">reset</VBtn>
    </p>
    <p>
      <VBtn @click="stopTimer">stop</VBtn>
    </p>
  </VCard>
</template>

<script lang="ts" setup>

import { ref, onMounted } from "vue"

const remain = ref(0)


const props = defineProps({
  id: String
})

const socket = new WebSocket(`ws://localhost:8080/timer/${props.id}`)




const initTimer = () => {
  fetch(`http://localhost:8080/init/${props.id}`)
}

onMounted(() => {
  initTimer()

  socket.onmessage = (event) => {
    console.log(remain.value)
    remain.value = event.data
  }
})


const readTimer = () => {
  fetch(`http://localhost:8080/read/${props.id}`)
    .then((res) => res.json())
    .then((data) => {
      remain.value = data
    })
}

const startTimer = () => {
  fetch(`http://localhost:8080/start/${props.id}`)
}

const restartTimer = () => {
  fetch(`http://localhost:8080/restart/${props.id}`)
}

const resetTimer = () => {
  fetch(`http://localhost:8080/reset/${props.id}`)
}

const stopTimer = () => {
  fetch(`http://localhost:8080/stop/${props.id}`)
}
</script>
