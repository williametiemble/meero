<template>
  <div id="app">
    <b-navbar toggleable="md" type="dark" variant="primary">

      <b-navbar-brand href="#">THE CAT WEBSITE</b-navbar-brand>

        <b-navbar-nav class="ml-auto">
          <b-button v-b-modal.modalFavorite class="float-right" variant="primary">
            Mes favoris
          </b-button>
        </b-navbar-nav>

    </b-navbar>
    <b-container fluid>
      <b-row class="mt-3 mb-3">
        <b-col cols="12" md="4" lg="4" v-for="(cat, n) in cats">
          <Card :cat="cat" @modalOpen="modalOpen(cat)" />
        </b-col>
      </b-row>
    </b-container> 

    <b-modal ref="modalCat" id="modalCat" size="lg" cancel-disabled ok-disabled>
      <b-container>
        <b-row>
          <b-col>
            <img :src="catDetail.image[0]" />
          </b-col>
          <b-col>
            <h3>{{catDetail.name}}</h3>
            <p>{{catDetail.description}}</p>
          </b-col>
        </b-row>
      </b-container>
      <div slot="modal-footer" class="w-100">
      </div>      
    </b-modal>        
  </div>
</template>

<script>
import axios from 'axios'
import Card from './components/Card.vue'

export default {
  name: 'app',
  components: {
    Card
  },
  data () {
    return {
      cats: [],
      catDetail: {
        image: [],
        name: '',
        description: ''
      }
    }
  },
  methods: {
    modalOpen(cat) {
      console.log(cat)
      this.catDetail = cat
      this.$refs.modalCat.show()
    }
  },
  mounted () {
    var _self = this    
    axios
      .get('https://api.thecatapi.com/v1/breeds')
      .then(
        function (response) {
          _self.cats = []
          response.data.forEach(function(element) {
              if (element.wikipedia_url) {
                var cat = {
                  id: element.id,
                  name: element.name,
                  origin: element.origin,
                  catTitle: element.wikipedia_url.replace("https://en.wikipedia.org/wiki/", ""),
                  image: []
                }
                axios
                  .get('https://en.wikipedia.org/api/rest_v1/page/media/'+cat.catTitle)
                  .then(
                    function (response) {
                      response.data.items.forEach(function(element) {
                          cat.image.push(element.thumbnail.source)
                        }
                      )
                      axios
                        .get('https://en.wikipedia.org/api/rest_v1/page/summary/'+cat.catTitle)
                        .then(
                          function (response) {
                            cat.description = response.data.extract
                            _self.cats.push(cat)
                          }
                        )
                    }
                  )
              }
          })
        }
      )
  }  
}
</script>

<style>

</style>
