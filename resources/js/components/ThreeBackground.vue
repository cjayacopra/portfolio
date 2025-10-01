<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue'
import * as THREE from 'three'

const container = ref<HTMLElement | null>(null)

onMounted(() => {
    if (!container.value) return

    let renderer: THREE.WebGLRenderer
    let scene: THREE.Scene
    let camera: THREE.PerspectiveCamera
    let shapes: THREE.Mesh[] = []
    let stars: THREE.Points

    // Shooting stars variables
    const shootingStarsArray: { mesh: THREE.Mesh, velocity: THREE.Vector3, life: number }[] = []
    const shootingStarGeometry = new THREE.SphereGeometry(0.05, 8, 8)
    const shootingStarMaterial = new THREE.MeshBasicMaterial({ color: 0xffffff })
    const maxShootingStars = 5
    const shootingStarLife = 150 // frames

    // Scene
    scene = new THREE.Scene()

    // Camera
    camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 2000)
    camera.position.z = 5

    // Renderer
    renderer = new THREE.WebGLRenderer({ antialias: true })
    renderer.setClearColor(0x000000, 1)
    renderer.setSize(window.innerWidth, window.innerHeight)
    container.value.appendChild(renderer.domElement)

    // Shapes
    const geometry = new THREE.IcosahedronGeometry(1, 0)
    const material = new THREE.MeshStandardMaterial({ color: 0x4f46e5, flatShading: true })

    for (let i = 0; i < 3; i++) {
        const mesh = new THREE.Mesh(geometry, material)
        mesh.position.x = (Math.random() - 0.5) * 10
        mesh.position.y = (Math.random() - 0.5) * 10
        mesh.position.z = (Math.random() - 0.5) * 10
        shapes.push(mesh)
        scene.add(mesh)
    }

    // Starfield
    const starVertices = []
    for (let i = 0; i < 10000; i++) {
        const x = (Math.random() - 0.5) * 2000
        const y = (Math.random() - 0.5) * 2000
        const z = (Math.random() - 0.5) * 2000
        starVertices.push(x, y, z)
    }
    const starGeometry = new THREE.BufferGeometry()
    starGeometry.setAttribute('position', new THREE.Float32BufferAttribute(starVertices, 3))
    const starMaterial = new THREE.PointsMaterial({ color: 0xffffff, size: 1 })
    stars = new THREE.Points(starGeometry, starMaterial)
    scene.add(stars)

    // Light
    const light = new THREE.DirectionalLight(0xffffff, 1)
    light.position.set(1, 1, 1)
    scene.add(light)

    // Function to create a shooting star
    function createShootingStar() {
        const startX = (Math.random() - 0.5) * 20
        const startY = (Math.random() - 0.5) * 20
        const startZ = (Math.random() - 0.5) * 20

        const endX = (Math.random() - 0.5) * 20
        const endY = (Math.random() - 0.5) * 20
        const endZ = (Math.random() - 0.5) * 20

        const velocity = new THREE.Vector3(endX - startX, endY - startY, endZ - startZ).normalize().multiplyScalar(0.2)

        const shootingStar = new THREE.Mesh(shootingStarGeometry, shootingStarMaterial)
        shootingStar.position.set(startX, startY, startZ)
        scene.add(shootingStar)

        shootingStarsArray.push({ mesh: shootingStar, velocity: velocity, life: shootingStarLife })
    }

    // Initial shooting stars
    for (let i = 0; i < 2; i++) {
        createShootingStar()
    }

    // Animation
    const animate = () => {
        requestAnimationFrame(animate)

        shapes.forEach(shape => {
            shape.rotation.x += 0.005
            shape.rotation.y += 0.005
        })

        if (stars) {
            stars.rotation.y += 0.0001
        }

        // Update shooting stars
        for (let i = shootingStarsArray.length - 1; i >= 0; i--) {
            const star = shootingStarsArray[i]
            star.mesh.position.add(star.velocity)
            star.life--

            if (star.life <= 0) {
                scene.remove(star.mesh)
                shootingStarsArray.splice(i, 1)
            }
        }

        // Randomly create new shooting stars
        if (Math.random() < 0.005 && shootingStarsArray.length < maxShootingStars) {
            createShootingStar()
        }

        renderer.render(scene, camera)
    }

    // Resize handler
    const onWindowResize = () => {
        camera.aspect = window.innerWidth / window.innerHeight
        camera.updateProjectionMatrix()
        renderer.setSize(window.innerWidth, window.innerHeight)
    }
    window.addEventListener('resize', onWindowResize)

    animate()

    onUnmounted(() => {
        window.removeEventListener('resize', onWindowResize)
        if (container.value && renderer.domElement) {
            container.value.removeChild(renderer.domElement)
        }
    })
})
</script>

<template>
    <div ref="container" class="fixed top-0 left-0 w-full h-full"></div>
</template>
