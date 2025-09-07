<script setup lang="ts">
import { ref, watch } from 'vue'

import soundUrl from '@/assets/sounds/chrrp.mp3'

// const player = ref<HTMLAudioElement>()
const player = new Audio(soundUrl)
player.autoplay = false
player.volume = 1
player.playbackRate = 1

let timer: ReturnType<typeof setInterval>
const interval = ref<number>(3)
const volume = ref<number>(1)
const isPlaying = ref(false)

watch(isPlaying, async (playStatus) => {
  if (playStatus == true) {
    console.log("Watch start - interval " + interval.value)
    startPlayer(interval.value)
  }
  else {
    console.log("Watch stop: " + isPlaying.value)
    stopPlayer()
  }
})

watch(interval, async (newInterval) => {
  if (isPlaying.value) {
    clearInterval(timer)
    startPlayer(newInterval)
  }
})

watch(volume, async (newVolume) => {
  player.volume = newVolume / 100
})
const playSound = () => {
  console.log("Play sound")
  player.currentTime = 0
  player.play()
}

const stopPlayer = () => {
  console.log("Stop player")
  clearInterval(timer)
  player.pause()
}

const startPlayer = (time: number) => {
  console.log("Start player. Interval: " + time)
  playSound()
  timer = setInterval(playSound, time * 1000)
}

// onMounted(() => {
//   player.value?.play();
// })

</script>

<template>
  <div class="flex flex-col">
    <label for="interval-control">Interval in seconds</label>
    <input v-model="interval" id="interval-control" type="range" min="1" max="100"
      class="text-base font-medium text-gray-500 mb-1">
    <div>{{ interval }}</div>
    <label for="volume">Volume</label>
    <input v-model="volume" id="volume" type="range" min="1" max="100" class="text-base font-medium text-gray-500 mb-1">
    <div>{{ volume }}</div>
    <button @click="isPlaying = !isPlaying" class="bg-gray-400">
      {{ isPlaying ? 'Stop' : 'Start' }}
    </button>
  </div>
</template>

<style scoped></style>
