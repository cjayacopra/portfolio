<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits(['navigate', 'viewProject'])

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
    isProjectsSubmenuOpen.value = false
    emit('navigate', section)
}

function toggleProjectsSubmenu() {
    isProjectsSubmenuOpen.value = !isProjectsSubmenuOpen.value
}

function selectProject(project: any) {
    isProjectsSubmenuOpen.value = false
    emit('viewProject', project)
}

</script>

<template>
    <nav class="fixed top-1/2 right-16 -translate-y-1/2 z-50 hidden md:block">
        <div class="relative p-4">
            <!-- Corner Border -->
            <div class="absolute top-0 right-0 h-1/4 w-px bg-gradient-to-b from-indigo-500 to-transparent"></div>
            <div class="absolute top-0 right-0 w-1/4 h-px bg-gradient-to-l from-indigo-500 to-transparent"></div>

            <ul class="space-y-4 text-left">
                <li v-for="link in navLinks" :key="link.name">
                    <button @click="navigate(link.section)" class="text-gray-400 hover:text-white transition-colors duration-300 group">
                        <span class="font-mono text-sm">{{ link.name }}</span>
                    </button>
                </li>

                <!-- Projects with Submenu -->
                <li class="relative">
                    <button @click="toggleProjectsSubmenu" class="text-gray-400 hover:text-white transition-colors duration-300 group">
                        <span class="font-mono text-sm">Projects</span>
                    </button>
                    <Transition name="submenu">
                        <div v-if="isProjectsSubmenuOpen" class="absolute right-full mr-4 -mt-8 w-48 bg-black/30 backdrop-blur-xl border border-white/10 rounded-lg shadow-xl py-2">
                            <ul>
                                <li v-for="project in projects" :key="project.id">
                                    <button @click="selectProject(project)" class="w-full text-left px-4 py-2 text-sm text-gray-300 hover:bg-indigo-600 hover:text-white transition-colors duration-200">
                                        {{ project.name }}
                                    </button>
                                </li>
                            </ul>
                        </div>
                    </Transition>
                </li>

                <!-- Resume Link -->
                <li>
                    <a :href="resumeLink.href" target="_blank" rel="noopener noreferrer" class="text-gray-400 hover:text-white transition-colors duration-300 group font-mono text-sm">
                        {{ resumeLink.name }}
                    </a>
                </li>
            </ul>
        </div>
    </nav>
</template>

<style scoped>
.submenu-enter-active,
.submenu-leave-active {
    transition: all 0.3s ease-out;
    transform-origin: top left;
}

.submenu-enter-from,
.submenu-leave-to {
    opacity: 0;
    transform: scale(0.9) translateY(-10px);
}
</style>
