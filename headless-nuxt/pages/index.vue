<template>


<div v-if="projects">
  <h1>Portfolio</h1>
  <div v-if="types">
                <button  @click="filterSelection('all')">All</button>
                    <button  v-for="projectType in types" :key="projectType" @click="filterSelection(projectType)" >
                    {{ projectType }}
                    </button>
                </div>
            
  <nuxt-link :to="`projects/${project.slug}`" v-for="project in filteredSelection" :key="project.id" >
  <button style="display: block; text-decoration: none !important;">{{ project.name}}</button>
</nuxt-link>
</div>


</template>

<script setup>

const {find} = useStrapi()
const projects = ref()

const types = ref([])

const activeFilter = ref('all')

const filterSelection = (type) => {
    activeFilter.value = type
}
const filteredSelection = computed(() => {
    if(activeFilter.value === 'all') return projects.value.data
    return projects.value.data.filter(project => project.type === activeFilter.value)
})

onMounted( async() => {
  projects.value = await find('projets', {populate :'deep'})
  types.value = new Set(projects.value.data.map(project => project.type))

})

 
</script>

<style scoped>


</style>