<template>
  <section>
    <h2>{{ title }}</h2>
    <h3>${{ price}}</h3>
    <p>{{ description}}</p>
    <router-link to="/products/p2">Product 2</router-link>
  </section>
</template>

<script>
import {  inject, computed  } from 'vue';
import { useRoute } from 'vue-router';
//the functions above were written to work with composition API. THey are called composebles.
//the useRoute allows us to use the route.object.


export default {
  props: ['pid'],
  setup() {
    const products = inject('products');
    // const title = ref('');
    // const price = ref(null);
    // const description = ref('');


    //#### another way to work with router is using the useRoute composable ####

    const route = useRoute();
    console.log(route)

    //creating computed for all these data to keep the page responsive when pressing a button to product 2.
    const selectedProduct = computed(() => products.value.find(product => product.id === route.params.pid)); 
    const title = computed(() => selectedProduct.value.title);
    const price = computed(() => selectedProduct.value.price);
    const description = computed(() => selectedProduct.value.description);
    //in the optional API, the router info can be accessed by the 
    // this.$route
    //but in the setup() method, there is no this. keyword.
    // but it can be done by setting the dynamic segment being passed as a props.
    //{ path: '/:detail', component: Component, props: true}
    //the :detail dynamic will be catch by props and used in the setup(props)
    //you can inject the info from the app.vue to the component.

    return { title, price, description, selectedProduct };
  },
};
</script>


<style scoped>
section {
  margin: 3rem auto;
  max-width: 40rem;
  padding: 1rem;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
}
</style>