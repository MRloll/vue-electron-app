<script setup lang="ts">
import { useRouter } from 'vue-router'

defineProps<{ variant: 'home' | 'portfolio' }>()

const router = useRouter()

const scrollTo = (id: string): void => {
  document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' })
}
</script>

<template>
  <UContainer
    as="nav"
    class="fixed top-0 left-0 right-0 z-50 backdrop-blur-md bg-neutral-950/70 border-b border-neutral-800"
    :ui="{ base: 'max-w-6xl mx-auto px-6 h-16 flex items-center justify-between' }"
  >
    <span
      class="text-xl font-bold bg-gradient-to-r from-indigo-400 to-purple-400 bg-clip-text text-transparent"
    >
      &lt;Dev /&gt;
    </span>
    <div class="flex items-center gap-2">
      <template v-if="variant === 'home'">
        <UButton label="Portfolio" color="primary" variant="ghost" @click="router.push('/portfolio')" />
      </template>
      <template v-else>
        <UButton
          v-for="item in ['About', 'Skills', 'Projects', 'Contact']"
          :key="item"
          :label="item"
          color="neutral"
          variant="ghost"
          @click="scrollTo(item.toLowerCase())"
        />
        <USeparator orientation="vertical" class="h-6 mx-1" />
        <UButton label="Home" color="neutral" variant="ghost" @click="router.push('/')" />
      </template>
    </div>
  </UContainer>
</template>
