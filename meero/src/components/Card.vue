<template>
  <div class="card">
    <b-card :title="cat.name"
            :img-src="cat.image[0]"
            :img-alt="cat.name"
            img-top
            class="mb-2">
      <p class="card-text">
        {{cat.origin}}
      </p>
      <b-button variant="primary" @click="modalOpen" class="btn float-right" size="sm">Detail</b-button>      
    </b-card>  
  </div>
</template>

<script>
export default {
  name: 'Card', 
  methods: {
    modalOpen() {
      this.$emit('modalOpen')
    }
  },   
  props: {
    cat: Object
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .card-img-top {
      width: 100%;
      height: 15vw;
      object-fit: cover;
      object-position: top;
  }
</style>
