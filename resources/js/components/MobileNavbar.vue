<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits(['navigate', 'viewProject'])

const isOpen = ref(false)
const isProjectsSubmenuOpen = ref(false)

const navLinks = [
    { name: 'Home', section: 'Home' },
    { name: 'About', section: 'About' },
    { name: 'Skills', section: 'Skills' },
    { name: 'Contact', section: 'Contact' },
]

const projects = [
    { id: 1, name: 'Project Alpha', description: 'An advanced e-commerce platform using Laravel.' },
    { id: 2, name: 'Project Beta', description: 'A high-performance API server with NestJS.' },
    { id: 3, name: 'Project Gamma', description: 'A serverless application on the edge with Hono.' },
]

const resumeLink = { name: 'Resume', href: 'https://rxresu.me/cjayacopra/dev-ops' }

function navigate(section: string) {
    isOpen.value = false
    isProjectsSubmenuOpen.value = false
    emit('navigate', section)
}

function selectProject(project: any) {
    isOpen.value = false
    isProjectsSubmenuOpen.value = false
    emit('viewProject', project)
}
</script>

<template>
    <div class="md:hidden fixed top-6 right-6 z-50">
        <!-- Hamburger Icon -->
        <button @click="isOpen = true" class="text-white">
            <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"></path>
            </svg>
        </button>

        <!-- Menu Overlay -->
        <Transition name="slide-from-right">
            <div v-if="isOpen" class="fixed top-0 bottom-0 right-0 w-3/4 max-w-sm bg-black/80 backdrop-blur-lg p-6">
                <!-- Close Button -->
                <button @click="isOpen = false" class="absolute top-6 right-6 text-white">
                    <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                    </svg>
                </button>

                <nav class="h-full flex flex-col justify-center">
                    <ul class="space-y-8 text-center">
                        <li v-for="link in navLinks" :key="link.name">
                            <button @click="navigate(link.section)" class="text-3xl font-light text-gray-300 hover:text-white">
                                {{ link.name }}
                            </button>
                        </li>
                        <li>
                            <button @click="isProjectsSubmenuOpen = !isProjectsSubmenuOpen" class="text-3xl font-light text-gray-300 hover:text-white">
                                Projects
                            </button>
                            <Transition name="submenu">
                                <ul v-if="isProjectsSubmenuOpen" class="mt-4 space-y-4">
                                    <li v-for="project in projects" :key="project.id">
                                        <button @click="selectProject(project)" class="text-xl font-light text-gray-400 hover:text-white">
                                            {{ project.name }}
                                        </button>
                                    </li>
                                </ul>
                            </Transition>
                        </li>
                        <li>
                            <a :href="resumeLink.href" target="_blank" rel="noopener noreferrer" class="text-3xl font-light text-gray-300 hover:text-white">
                                {{ resumeLink.name }}
                            </a>
                        </li>
                    </ul>
                </nav>
            </div>
        </Transition>
    </div>
</template>

<style scoped>
.slide-from-right-enter-active,
.slide-from-right-leave-active {
    transition: transform 0.3s ease-out;
}
.slide-from-right-enter-from,
.slide-from-right-leave-to {
    transform: translateX(100%);
}

.submenu-enter-active,
.submenu-leave-active {
    transition: all 0.3s ease-out;
}
.submenu-enter-from,
.submenu-leave-to {
    opacity: 0;
    transform: translateY(-10px);
}
</style>
