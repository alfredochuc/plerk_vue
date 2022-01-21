<template>
  <div class="hello">
      <div class="container">
        <h1>Lista de Beneficios</h1>
        Buscar un beneficio : 
        <input type="text"  v-model="searchQuery" placeholder="Buscar">
      </div>
      <div v-if="resultQuery.length" >

        <div class="container-categories">
          <div v-for='category in resultQuery' :key="category.uuid4" class="card" >
            <div class="card-img">
              <img :src="category.image" class="img-responsive" :alt="category.name.esp">
            </div>
            <div class="card-body">
              <h1>{{category.name.esp | capitalize}}</h1>
              <p>{{category.description.esp}}</p>
              <div class="row-type-price">
                <span><b>Tipo:</b> {{category.category_type | category_type }}</span>
                <span><b>Precio:</b> {{category.suggested_budget | currency}}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-else>
        <div class="warning">No hay categorías</div>
      </div>
  </div>
</template>

<script>
import axios from "axios"
// axios.defaults.headers.common['Authorization'] = '3d49fb0924d5b5a86cf5fcc66e31347f0bef65715ccb66f59a30b6ffe33d1e86f15040a40e16b11f26cb82940c9d4d988a074d8d1d82c4a3fba077dafd34844f';
export default {
  name: 'CatalogCategories',
  props: {
    msg: String
  },
  data() {
    return {
      categories:[],
      searchQuery: '',
      categoryTypes: ['Normal','Libre','Personalizada']
    }
  },
  created() {
    axios.get('https://apitesting.plerk.io/v2/category', {
      headers: {
        'Authorization': `5bc95bf034d900548243a59e2296bd683729bd75057879fd0f877d3adc7d1db6bedbfccb47aca04e44ef28adf4c3e9e72afe2f2b295b3bf08e2a47ec75f9607d`
      }
    })
    .then((res) => {
      this.categories = res.data.data;
    })
    .catch((error) => {
      console.error(error)
    })
  },
  computed: {
    resultQuery() {
      return this.categories.filter(category => {
        return category.name.esp.toLowerCase().includes(this.searchQuery.toLowerCase())
      })
    }
  },
  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    },
    category_type: function (value) {
      if (!value) return ''
      switch (value) {
        case 1:
          return 'Normal';
        case 2:
          return 'Libre';
        case 3:
          return 'Personalizada';
        default:
          return '';
      }
    },
    currency: function (value) {
      if (!value) return ''
      // return "$ " + value.toFixed(2).toLocaleString() + " USD";

       if (typeof value !== "number") {
          return value;
      }
      var formatter = new Intl.NumberFormat('en-US', {
          style: 'currency',
          currency: 'USD'
      });
      return formatter.format(value);

    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1{
  font-size: 2rem;
}
.card h1{
  font-size: 25px;
}
.container-categories{
  width: 1240px;
  margin: 0 auto;
  margin-top: 50px;
  display: grid;
  grid-template-columns: repeat(4, minmax(200px, 1fr));
  /* grid-template-rows: repeat(2, 200px); */
  grid-gap: 2rem;
}
.img-responsive{
  width: 80%;
  height: auto;
  object-fit: cover;
  border-radius: 50%;
}
.card{
  box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 12px;
  padding: 20px;
}
.card .row-type-price{
  display: flex;
  justify-content: space-between;
  /* align-items: ; */
}
.card-body{
  overflow: hidden;
}

.warning{
  margin-top: 40px;
  background-color: burlywood;
  padding: 15px 20px;
  border-radius: 10px;
  font-weight: 500;
}

@media only all and (max-width: 1024px) {
  .container-categories{
    width: 90%;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  }
}
</style>
