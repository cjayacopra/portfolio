<script setup lang="ts">
import { Head } from '@inertiajs/vue3'
import VerticalNavbar from '../components/VerticalNavbar.vue'
import MobileNavbar from '../components/MobileNavbar.vue'
import { ref, onMounted, shallowRef } from 'vue'

const activeSection = ref('Home')
const activeProject = ref<any>(null)
const ThreeBackground = shallowRef(null)

onMounted(() => {
    import('../components/ThreeBackground.vue').then(module => {
        ThreeBackground.value = module.default
    })
})

function navigate(section: string) {
    activeSection.value = section
    activeProject.value = null // Reset project view when changing main section
}

function showProject(project: any) {
    activeSection.value = 'Projects'
    activeProject.value = project
}

</script>

<template>
    <Head>
        <title>Portfolio</title>
    </Head>

    <div class="text-white font-sans w-screen h-screen overflow-hidden">
        <component :is="ThreeBackground" />
        
        <!-- Navigation -->
        <VerticalNavbar @navigate="navigate" @viewProject="showProject" />
        <MobileNavbar @navigate="navigate" @viewProject="showProject" />

        <main class="w-full h-full flex items-center justify-center">
            <Transition name="fade" mode="out-in">
                <div :key="activeSection + (activeProject ? activeProject.id : '')" class="relative w-full max-w-5xl px-6 sm:px-8 text-center pt-12 pb-12">
                        <!-- Bottom-Left Corner Border -->
                        <div class="absolute bottom-0 left-0 h-1/4 w-px bg-gradient-to-t from-indigo-500 to-transparent"></div>
                        <div class="absolute bottom-0 left-0 w-1/4 h-px bg-gradient-to-r from-indigo-500 to-transparent"></div>

                        <!-- Top-Right Corner Border (Mobile) -->
                        <div class="absolute top-0 right-0 h-1/4 w-px bg-gradient-to-b from-indigo-500 to-transparent md:hidden"></div>
                        <div class="absolute top-0 right-0 w-1/4 h-px bg-gradient-to-l from-indigo-500 to-transparent md:hidden"></div>

                        <!-- Home -->
                        <div v-if="activeSection === 'Home'">
                            <h1 class="text-4xl sm:text-5xl md:text-6xl font-bold">Christopher Jay D. Acopra</h1>
                            <p class="mt-4 text-lg md:text-xl text-gray-300">Engineering scalable, high-performance web solutions.</p>
                            <div class="mt-8 flex justify-center items-center space-x-6">
                                <a href="https://github.com/cjayacopra" target="_blank" rel="noopener noreferrer" class="text-gray-400 hover:text-white transition-colors duration-300">
                                    <svg class="w-8 h-8 inline-block" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor"><path fill-rule="evenodd" d="M12 2C6.477 2 2 6.484 2 12.017c0 4.425 2.865 8.18 6.839 9.504.47.087.677-.233.677-.522 0-.256-.009-.93-.014-1.82-2.782.603-3.37-1.34-3.37-1.34-.454-1.158-1.11-1.465-1.11-1.465-.908-.62.069-.608.069-.608 1.004.07 1.532 1.03 1.532 1.03.892 1.53 2.341 1.088 2.91.832.091-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.95 0-1.091.39-1.984 1.03-2.682-.104-.253-.448-1.27.097-2.646 0 0 .84-.27 2.75 1.025.798-.222 1.649-.333 2.5-.333.85 0 1.702.111 2.5.333 1.909-1.295 2.747-1.025 2.747-1.025.546 1.376.202 2.393.097 2.646.64.698 1.026 1.591 1.026 2.682 0 3.846-2.339 4.692-4.566 4.942.359.31.678.923.678 1.855 0 1.338-.012 2.419-.012 2.747 0 .292.206.615.682.52C21.137 20.195 24 16.44 24 12.017 24 6.484 19.522 2 14 2h-2z" clip-rule="evenodd"/></svg>
                                </a>
                                <a href="https://linkedin.com/in/your-username" target="_blank" rel="noopener noreferrer" class="text-gray-400 hover:text-white transition-colors duration-300">
                                    <svg class="w-8 h-8 inline-block" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor"><path d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"/></svg>
                                </a>
                            </div>
                        </div>
                        
                        <!-- About -->
                        <div v-if="activeSection === 'About'">
                            <h2 class="text-3xl md:text-4xl font-bold">About Me</h2>
                            <p class="mt-6 text-base md:text-lg leading-relaxed">
                                A dedicated software engineer with a passion for building robust and performant web applications. Leveraging expertise in backend technologies like Laravel and NestJS alongside modern frontend frameworks like Vue.js, I architect and develop solutions designed for efficiency and impact. I thrive on tackling complex challenges and am deeply committed to writing clean, maintainable code that drives innovation.
                            </p>
                        </div>

                        <!-- Skills -->
                        <div v-if="activeSection === 'Skills'">
                            <h2 class="text-3xl md:text-4xl font-bold">My Tech Stack</h2>
                            <div class="grid grid-cols-2 md:grid-cols-4 gap-4 md:gap-6 mt-10 text-center">
                                <div class="border border-gray-700 p-4 rounded-lg"><h3 class="text-base sm:text-lg md:text-xl font-semibold">Vue.js</h3></div>
                                <div class="border border-gray-700 p-4 rounded-lg"><h3 class="text-base sm:text-lg md:text-xl font-semibold">React</h3></div>
                                <div class="border border-gray-700 p-4 rounded-lg"><h3 class="text-base sm:text-lg md:text-xl font-semibold">Tailwind CSS</h3></div>
                                <div class="border border-gray-700 p-4 rounded-lg"><h3 class="text-base sm:text-lg md:text-xl font-semibold">Laravel</h3></div>
                                <div class="border border-gray-700 p-4 rounded-lg"><h3 class="text-base sm:text-lg md:text-xl font-semibold">Hono</h3></div>
                                <div class="border border-gray-700 p-4 rounded-lg"><h3 class="text-base sm:text-lg md:text-xl font-semibold">NestJS</h3></div>
                                <div class="border border-gray-700 p-4 rounded-lg"><h3 class="text-base sm:text-lg md:text-xl font-semibold">Node.js</h3></div>
                                <div class="border border-gray-700 p-4 rounded-lg"><h3 class="text-base sm:text-lg md:text-xl font-semibold">MySQL</h3></div>
                                <div class="border border-gray-700 p-4 rounded-lg"><h3 class="text-base sm:text-lg md:text-xl font-semibold">PostgreSQL</h3></div>
                                <div class="border border-gray-700 p-4 rounded-lg"><h3 class="text-base sm:text-lg md:text-xl font-semibold">Linux</h3></div>
                                <div class="border border-gray-700 p-4 rounded-lg"><h3 class="text-base sm:text-lg md:text-xl font-semibold">Docker</h3></div>
                                <div class="border border-gray-700 p-4 rounded-lg"><h3 class="text-base sm:text-lg md:text-xl font-semibold">Git</h3></div>
                            </div>
                        </div>

                        <!-- Projects -->
                        <div v-if="activeSection === 'Projects' && activeProject">
                            <h2 class="text-3xl md:text-4xl font-bold">{{ activeProject.name }}</h2>
                            <p class="mt-6 text-base md:text-lg">{{ activeProject.description }}</p>
                        </div>

                        <!-- Contact -->
                        <div v-if="activeSection === 'Contact'">
                            <h2 class="text-3xl md:text-4xl font-bold">Get In Touch</h2>
                            <p class="mt-6 text-base md:text-lg max-w-xl mx-auto">
                                Have a project in mind or just want to say hello? I'd love to hear from you.
                            </p>
                            <div class="mt-8">
                                <a href="mailto:cjayacopra@gmail.com" class="text-xl md:text-2xl font-bold text-indigo-400 hover:text-indigo-300 transition-colors duration-300">
                                    cjayacopra@gmail.com
                                </a>
                            </div>
                        </div>
                </div>
            </Transition>
        </main>
    </div>
</template>

<style>
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}
</style>

