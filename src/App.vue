<script setup>
import { Octokit } from "https://cdn.skypack.dev/@octokit/core";
import config from '../config'
import { ref, onMounted } from 'vue'
import Prs from './components/prs.vue'
let __config
try {
    const xhr = new XMLHttpRequest()
    xhr.open('GET', './.config.json', false)
    xhr.send()
    __config = JSON.parse(xhr.responseText)
} catch(e) {}
if (__config) Object.assign(config, __config)

const octokit = new Octokit({
  auth: config.token
})

const res = ref({})

onMounted(() => {
  octokit.request('GET /repos/{owner}/{repo}/pulls', {
    owner: 'FE-star',
    repo: config.repo
  }).then(v => {
    res.value = v
  })
})
</script>

<template>
  <main>
    <Prs :data="res.data" :nameMap="config.nameMap" :startTime="config.startTime" />
  </main>
</template>

<style scoped>
</style>
