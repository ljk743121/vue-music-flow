<template>
  <main>
    <h1 class="mw:bg-neutral-950 mw:p-4 mw:text-3xl">Playground</h1>
    <div>
      <ul class="mw:space-y-4 mw:bg-neutral-800 mw:p-8">
        <li class="mw:mb-6 mw:font-semibold mw:text-orange-400">Play single track example</li>
        <li v-for="track in tracks" :key="track.id" class="mw:flex mw:items-center mw:gap-4">
          <button
            class="mw:flex mw:cursor-pointer mw:items-center mw:justify-center mw:hover:text-orange-500"
            type="button"
            @click="onPlaySingleTrack(track)"
          >
            <IconPlay class="mw:size-6" v-if="!isTrackPlaying(track.id)" />
            <IconPause class="mw:size-6" v-else />
          </button>
          <span>{{ track.title }}</span>
        </li>
      </ul>
      <hr class="mw:border-neutral-600" />
      <ul class="mw:space-y-4 mw:bg-neutral-800 mw:p-8">
        <li class="mw:mb-6 mw:font-semibold mw:text-orange-400">Play track as playlist example</li>
        <li v-for="track in tracks" :key="track.id" class="mw:flex mw:items-center mw:gap-4">
          <button
            class="mw:flex mw:cursor-pointer mw:items-center mw:justify-center mw:hover:text-orange-500"
            type="button"
            @click="onPlayAsPlaylist(tracks, track)"
          >
            <IconPlay class="mw:size-6" v-if="!isTrackPlaying(track.id)" />
            <IconPause class="mw:size-6" v-else />
          </button>
          <span>{{ track.title }}</span>
        </li>
      </ul>
    </div>
    <AudioPlayer
      :options="{
        // waveColor: '#ffffff',
        // progressColor: '#ffa045',
        // height: 50,
        autoplay: true,
      }"
      :fetchUrl = "fetchTracks"
    >
      <template #actions="{ track }">
        <button
          type="button"
          class="mw:flex mw:size-10 mw:cursor-pointer mw:items-center mw:justify-center mw:rounded mw:p-2 mw:hover:text-red-500"
          aria-label="Mark as favorite"
          @click="onMarkTrackAsFavorite(track)"
        >
          <IconHeart class="mw:size-10" />
        </button>
      </template>
    </AudioPlayer>
  </main>
</template>

<script setup lang="ts">
import AudioPlayer from './components/AudioPlayer.vue'
import usePlayer, { type TPlayerTrack } from './composables/usePlayer'
import { ref } from 'vue'
import IconHeart from './components/Icons/IconHeart.vue'
import IconPlay from './components/Icons/IconPlay.vue'
import IconPause from './components/Icons/IconPause.vue'

defineOptions({
  name: 'PagePlayground',
})

const { isTrackPlaying, onPlaySingleTrack, onPlayAsPlaylist } = usePlayer()

const onMarkTrackAsFavorite = (track: TPlayerTrack | null) => {
  console.log(track)
  alert(`Market track ${track?.title} as favorite`)
}

async function fetchTracks(data: Record<string, unknown>) {
  let response = ''
  if (data.songId==='123456'){
    response = 'http://m701.music.126.net/20250912215047/84beef6a08b9eeec67c806f9975178b0/jdymusic/obj/wo3DlMOGwrbDjj7DisKw/32404374480/3803/10e2/4f6d/034625c58bc75a7d2d9846ed41ec3ca5.mp3?vuutv=6pA8JF8xmYIY+ybvl6tzMlo13FPQ+6aqxNbYaqBK/Wz9FaXEEDs0dpZvodu+yPTIOeuN05SNMoy8VAPHqb866hMHahlMLJOShj9W1nYWKVL3MrwOYzgZIN5vZotpeoVUJqfC00uHp//Uuhgm7m2nVw==&cdntag=bWFyaz1vc193ZWIscXVhbGl0eV9zdGFuZGFyZA';
  }
  return response
}

const tracks = ref<TPlayerTrack[]>([])
fetch('/data.json')
  .then((response) => response.json())
  .then((data) => (tracks.value = data))
</script>

<style>
body {
  background: #181818;
  color: #ffffff;
}
</style>
