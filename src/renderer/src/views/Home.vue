<script setup lang="ts">
import NavBar from '../components/portfolio/NavBar.vue'
</script>

<template>
  <div class="min-h-screen bg-neutral-950 text-white overflow-x-hidden">
    <NavBar variant="home" />

    <div class="flex flex-col items-center justify-center min-h-screen px-6 text-center">
      <div class="max-w-2xl mx-auto">
        <span class="text-sm tracking-widest uppercase text-indigo-400 mb-4 block"> Welcome </span>
        <h1 class="text-5xl md:text-7xl font-bold mb-6">
          Electron + Vue
          <span
            class="bg-gradient-to-r from-indigo-400 via-purple-400 to-pink-400 bg-clip-text text-transparent"
          >
            Starter
          </span>
        </h1>
        <p class="text-lg text-neutral-400 mb-8 leading-relaxed">
          A modern desktop application built with Electron, Vue 3, TypeScript, Nuxt UI, TresJS 3D,
          and GSAP animations.
        </p>
        <div class="flex items-center justify-center gap-4">
          <UButton
            label="View Portfolio"
            color="primary"
            size="lg"
            @click="$router.push('/portfolio')"
          />
          <UButton
            label="GitHub"
            color="neutral"
            variant="outline"
            size="lg"
            href="https://github.com/MRloll/vue-electron-app"
            target="_blank"
          />
        </div>
      </div>
    </div>

    <footer class="py-8 px-6 border-t border-neutral-800">
      <div class="max-w-6xl mx-auto text-center">
        <p class="text-sm text-neutral-500">&copy; 2026 Web Developer. All rights reserved.</p>
      </div>
    </footer>
  </div>
</template>
