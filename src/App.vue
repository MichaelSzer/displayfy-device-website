<script setup>
import { ref } from 'vue'
import Setup from './components/Setup.vue'
import Connecting from './components/Connecting.vue'

const done = ref(false)

</script>

<template>
  <Setup v-if="!done" @setupDone="done = true"/>
  <Connecting v-else />
</template>

<style>
@import './assets/base.css';
</style>
