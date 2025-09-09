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
const volume = ref<number>(1 * 100)
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
    <div class="flex flex-col p-3">
      <div class="flex justify-between">
        <label for="interval-control">Interval</label>
        <span>{{ interval }} seconds</span>
      </div>
      <input v-model="interval" id="interval-control" type="range" min="1" max="100"
        class="range rounded-full border-0 bg-gray-200 accent-black text-base font-medium text-gray-500 m-0 p-0">
      <div class="flex justify-between text-xs">
        <span>1</span>
        <span>100</span>
      </div>
    </div>
    <div class="flex flex-col p-3">
      <div class="flex justify-between">
        <label for="volume-control">Volume</label>
        <span>{{ volume }}%</span>
      </div>
      <input v-model="volume" id="volume-control" type="range" min="1" max="100"
        class="range rounded-full border-0 bg-gray-200 accent-black text-base font-medium text-gray-500 m-0 p-0">
      <div class="flex justify-between text-xs">
        <span>1</span>
        <span>100</span>
      </div>
    </div>
    <div class="flex flex-col items-center">
      <button @click="isPlaying = !isPlaying"
        class="py-2 px-8 rounded-xl bg-transparent border-black border-3 text-xl font-bold">
        {{ isPlaying ? 'Stop' : 'Start' }}
      </button>
    </div>
  </div>
</template>

<style scoped></style>
