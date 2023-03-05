<template>
    <div v-if="project">
       <p>{{ project.name }}</p> 
       <p>{{ project.description }}</p>
        <img :src="project.image.url" alt="Image du projet" style="width: 300px; height: auto;">
        <div v-for="technologie in project.technologies" :key="technologie.id" style="display : flex">
            <p>{{ technologie.name }}</p>
            <img :src="technologie.image.url" alt="image du projet" style="width : 8%">
        </div>
        <button @click="Back()">Retour</button>
    </div>
</template>

<script setup>

const {findOne} = useStrapi()

const route = useRoute()

const project = ref()



onMounted(async () => {
    project.value = await findOne(`projets?filters[slug]=${route.params.slug}&populate=deep`)
    project.value = project.value.data[0]
    console.log(project.value)
})



</script>

<script>
export default {
  methods: {
    Back() {
      window.history.back();
    }
  }
}
</script>

<style scoped>


</style>