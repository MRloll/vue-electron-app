<script setup lang="ts">
import { shallowRef, watchEffect } from 'vue'
import * as THREE from 'three'

const canvasRef = shallowRef<HTMLCanvasElement | null>(null)
let scene: THREE.Scene | null = null
let camera: THREE.PerspectiveCamera | null = null
let renderer: THREE.WebGLRenderer | null = null
let torusKnot: THREE.Mesh | null = null
let particles: THREE.Points | null = null
let animationId: number | null = null

const init = (canvas: HTMLCanvasElement): void => {
  const w = canvas.clientWidth
  const h = canvas.clientHeight

  scene = new THREE.Scene()

  camera = new THREE.PerspectiveCamera(45, w / h, 0.1, 100)
  camera.position.set(0, 0, 8)

  renderer = new THREE.WebGLRenderer({ canvas, alpha: true, antialias: true })
  renderer.setSize(w, h)
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))

  const geometry = new THREE.TorusKnotGeometry(1.2, 0.4, 128, 16)
  const material = new THREE.MeshPhysicalMaterial({
    color: new THREE.Color('#6366f1'),
    metalness: 0.6,
    roughness: 0.2,
    clearcoat: 0.8,
    emissive: new THREE.Color('#6366f1'),
    emissiveIntensity: 0.1
  })
  torusKnot = new THREE.Mesh(geometry, material)
  scene.add(torusKnot)

  const ambientLight = new THREE.AmbientLight(0x404060)
  scene.add(ambientLight)

  const directionalLight = new THREE.DirectionalLight(0xffffff, 2)
  directionalLight.position.set(5, 5, 5)
  scene.add(directionalLight)

  const pointLight = new THREE.PointLight(0x818cf8, 3)
  pointLight.position.set(-3, -2, 4)
  scene.add(pointLight)

  const particleCount = 200
  const posArray = new Float32Array(particleCount * 3)
  for (let i = 0; i < particleCount * 3; i++) {
    posArray[i] = (Math.random() - 0.5) * 20
  }
  const particleGeo = new THREE.BufferGeometry()
  particleGeo.setAttribute('position', new THREE.BufferAttribute(posArray, 3))
  const particleMat = new THREE.PointsMaterial({
    color: 0x818cf8,
    size: 0.03,
    transparent: true,
    opacity: 0.6
  })
  particles = new THREE.Points(particleGeo, particleMat)
  scene.add(particles)

  const animate = (): void => {
    animationId = requestAnimationFrame(animate)
    if (torusKnot) {
      torusKnot.rotation.x += 0.005
      torusKnot.rotation.y += 0.01
    }
    if (renderer && scene && camera) {
      renderer.render(scene, camera)
    }
  }
  animate()
}

const cleanup = (): void => {
  if (animationId !== null) cancelAnimationFrame(animationId)
  if (renderer) {
    renderer.dispose()
    renderer = null
  }
  scene = null
  camera = null
  torusKnot = null
  particles = null
}

watchEffect((): (() => void) | undefined => {
  const canvas = canvasRef.value
  if (canvas) {
    cleanup()
    init(canvas)
  }
  return () => cleanup()
})
</script>

<template>
  <canvas ref="canvasRef" class="hero-3d-canvas" />
</template>

<style scoped>
.hero-3d-canvas {
  width: 100%;
  height: 100%;
  display: block;
}
</style>
