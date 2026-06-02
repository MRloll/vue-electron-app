<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import Hero3D from '../components/portfolio/Hero3D.vue'
import NavBar from '../components/portfolio/NavBar.vue'

gsap.registerPlugin(ScrollTrigger)

const scrollTo = (id: string): void => {
  document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' })
}

const ctx = ref<gsap.Context | null>(null)

const skills = [
  { name: 'Vue.js', level: 90, color: 'primary' },
  { name: 'React', level: 85, color: 'info' },
  { name: 'TypeScript', level: 88, color: 'primary' },
  { name: 'Node.js', level: 82, color: 'success' },
  { name: 'Three.js', level: 75, color: 'warning' },
  { name: 'GSAP', level: 78, color: 'error' },
  { name: 'Tailwind CSS', level: 92, color: 'info' },
  { name: 'Electron', level: 70, color: 'neutral' }
]

const progressColors: Record<string, string> = {
  primary: 'primary',
  info: 'info',
  success: 'success',
  warning: 'warning',
  error: 'error',
  neutral: 'neutral'
}

const projects = [
  {
    title: '3D Portfolio',
    desc: 'Interactive 3D portfolio built with TresJS and GSAP',
    tags: ['Vue', 'Three.js', 'GSAP']
  },
  {
    title: 'E-Commerce App',
    desc: 'Full-stack e-commerce platform with Vue and Node.js',
    tags: ['Vue', 'Node.js', 'MongoDB']
  },
  {
    title: 'Real-time Dashboard',
    desc: 'Analytics dashboard with real-time data visualization',
    tags: ['React', 'D3.js', 'Socket.io']
  },
  {
    title: 'Desktop App',
    desc: 'Cross-platform desktop app built with Electron',
    tags: ['Electron', 'Vue', 'SQLite']
  }
]

onMounted(() => {
  ctx.value = gsap.context(() => {
    ScrollTrigger.batch('.gsap-fade-up', {
      onEnter: (batch) => {
        gsap.fromTo(
          batch,
          { opacity: 0, y: 60 },
          { opacity: 1, y: 0, duration: 0.8, stagger: 0.15, ease: 'power3.out' }
        )
      },
      once: true
    })

    ScrollTrigger.batch('.gsap-scale-in', {
      onEnter: (batch) => {
        gsap.fromTo(
          batch,
          { opacity: 0, scale: 0.8 },
          { opacity: 1, scale: 1, duration: 0.6, stagger: 0.1, ease: 'back.out(1.7)' }
        )
      },
      once: true
    })

    gsap.fromTo(
      '.hero-title',
      { opacity: 0, y: 40 },
      { opacity: 1, y: 0, duration: 1, ease: 'power3.out' }
    )

    gsap.fromTo(
      '.hero-subtitle',
      { opacity: 0, y: 30 },
      { opacity: 1, y: 0, duration: 1, delay: 0.3, ease: 'power3.out' }
    )

    gsap.fromTo(
      '.hero-cta',
      { opacity: 0, y: 20 },
      { opacity: 1, y: 0, duration: 0.8, delay: 0.6, ease: 'power3.out' }
    )
  })
})

onUnmounted(() => {
  ctx.value?.revert()
})
</script>

<template>
  <div class="min-h-screen bg-neutral-950 text-white overflow-x-hidden">
    <NavBar variant="portfolio" />

    <section class="relative min-h-screen flex items-center justify-center overflow-hidden">
      <div class="absolute inset-0 z-0">
        <Hero3D />
      </div>
      <div
        class="absolute inset-0 bg-gradient-to-b from-transparent via-neutral-950/50 to-neutral-950 z-[1]"
      />
      <div class="relative z-10 text-center px-6">
        <p class="hero-subtitle text-sm tracking-widest uppercase text-indigo-400 mb-4">
          Web Developer &amp; Creative Coder
        </p>
        <h1 class="hero-title text-6xl md:text-8xl font-bold mb-6">
          Building
          <span
            class="bg-gradient-to-r from-indigo-400 via-purple-400 to-pink-400 bg-clip-text text-transparent"
            >immersive</span
          >
          <br />web experiences
        </h1>
        <div class="hero-cta flex items-center justify-center gap-4">
          <UButton label="View Projects" color="primary" size="lg" @click="scrollTo('projects')" />
          <UButton
            label="Get in Touch"
            color="neutral"
            variant="outline"
            size="lg"
            @click="scrollTo('contact')"
          />
        </div>
      </div>
      <div class="absolute bottom-10 left-1/2 -translate-x-1/2 z-10 animate-bounce">
        <UButton color="neutral" variant="ghost" size="xs" :trailing-icon="false">
          Scroll down
        </UButton>
      </div>
    </section>

    <section id="about" class="py-32 px-6">
      <UContainer class="max-w-4xl mx-auto gsap-fade-up">
        <UBadge color="primary" variant="soft" size="sm" class="mb-4">About</UBadge>
        <h2 class="text-4xl md:text-5xl font-bold mb-8">
          Crafting digital <span class="text-indigo-400">experiences</span>
        </h2>
        <p class="text-lg text-neutral-400 leading-relaxed mb-6">
          I'm a full-stack web developer passionate about creating beautiful, performant, and
          accessible web applications. With expertise in Vue.js, React, and Node.js, I bring ideas
          to life through clean code and thoughtful design.
        </p>
        <p class="text-lg text-neutral-400 leading-relaxed">
          When I'm not coding, you'll find me exploring new technologies, contributing to
          open-source projects, or experimenting with 3D graphics and animations.
        </p>
      </UContainer>
    </section>

    <section id="skills" class="py-32 px-6 bg-neutral-900/50">
      <UContainer class="max-w-6xl mx-auto">
        <div class="text-center mb-16 gsap-fade-up">
          <UBadge color="info" variant="soft" size="sm" class="mb-4">Skills</UBadge>
          <h2 class="text-4xl md:text-5xl font-bold">Tech stack</h2>
        </div>
        <div class="grid grid-cols-2 md:grid-cols-4 gap-6">
          <UCard
            v-for="skill in skills"
            :key="skill.name"
            class="gsap-scale-in"
            :ui="{
              background: 'bg-neutral-800/50 backdrop-blur-sm',
              ring: 'ring-1 ring-neutral-700/50',
              rounded: 'rounded-2xl',
              body: 'p-6'
            }"
          >
            <p class="font-semibold text-lg mb-4">{{ skill.name }}</p>
            <UProgress
              :model-value="skill.level"
              :max="100"
              :color="progressColors[skill.color]"
              size="sm"
              status
              :ui="{
                track: 'bg-neutral-700',
                indicator: 'bg-gradient-to-r from-indigo-500 to-purple-500',
                status: 'text-xs text-neutral-500 mt-1'
              }"
            />
          </UCard>
        </div>
      </UContainer>
    </section>

    <section id="projects" class="py-32 px-6">
      <UContainer class="max-w-6xl mx-auto">
        <div class="text-center mb-16 gsap-fade-up">
          <UBadge color="success" variant="soft" size="sm" class="mb-4">Projects</UBadge>
          <h2 class="text-4xl md:text-5xl font-bold">Selected work</h2>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <UCard
            v-for="project in projects"
            :key="project.title"
            class="gsap-scale-in group"
            :ui="{
              background: 'bg-neutral-800/30 backdrop-blur-sm',
              ring: 'ring-1 ring-neutral-700/50',
              rounded: 'rounded-2xl',
              body: 'p-8',
              shadow: 'shadow-none',
              divide: 'divide-none'
            }"
          >
            <template #header>
              <div
                class="w-12 h-12 rounded-xl bg-indigo-500/10 flex items-center justify-center mb-2 group-hover:bg-indigo-500/20 transition-colors"
              >
                <UIcon name="i-heroicons-code-bracket" class="text-2xl text-indigo-400" />
              </div>
            </template>
            <h3 class="text-2xl font-bold mb-3 text-black dark:text-neutral-200">
              {{ project.title }}
            </h3>
            <p class="text-neutral-400 mb-4">{{ project.desc }}</p>
            <div class="flex flex-wrap gap-2">
              <UBadge
                v-for="tag in project.tags"
                :key="tag"
                :label="tag"
                color="neutral"
                variant="outline"
                size="sm"
              />
            </div>
          </UCard>
        </div>
      </UContainer>
    </section>

    <section id="contact" class="py-32 px-6 bg-neutral-900/50">
      <UContainer class="max-w-3xl mx-auto text-center gsap-fade-up">
        <UBadge color="warning" variant="soft" size="sm" class="mb-4">Contact</UBadge>
        <h2 class="text-4xl md:text-5xl font-bold mb-6">Let's work together</h2>
        <p class="text-lg text-neutral-400 mb-10">
          Have a project in mind? Let's build something amazing together.
        </p>
        <UButton label="dev@example.com" color="primary" size="lg" icon="i-heroicons-envelope" />
      </UContainer>
    </section>

    <UFooter
      :ui="{
        wrapper: 'py-8 px-6 border-t border-neutral-800',
        container: 'max-w-6xl mx-auto'
      }"
    >
      <template #left>
        <p class="text-sm text-neutral-500">&copy; 2026 Web Developer. All rights reserved.</p>
      </template>
      <template #right>
        <div class="flex items-center gap-2">
          <UButton
            v-for="link in ['GitHub', 'LinkedIn', 'Twitter']"
            :key="link"
            color="neutral"
            variant="ghost"
            size="sm"
          >
            {{ link }}
          </UButton>
        </div>
      </template>
    </UFooter>
  </div>
</template>
