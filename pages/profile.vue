<template>
    <v-layout
    column
    justify-center
    align-center
    >
        <v-card
            class="mx-auto"
            max-width="544"
        >
            <v-img
            :src="data.image"
            height="500px"
            ></v-img>
    
            <v-card-title>
                <div class="name">{{data.name}}</div>
            </v-card-title>

            <v-card-subtitle>
                <div class="email">{{data.email}}</div>
            </v-card-subtitle>

        </v-card>
        <nuxt-link to="/">Выйти</nuxt-link>
    </v-layout>

</template>

<script>
  export default {
    data: () => ({
        data: {}
    }),
    async mounted() {
        this.data = await this.$axios.$get('data.json')
    }
  }
</script>