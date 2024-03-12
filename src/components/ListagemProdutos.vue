<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import { useScreen } from '@/composables/screen';
import Footer from '@/components/Footer.vue';

const { browserWidth, deviceWidth, isMobile } = useScreen();
const produtos = ref([]); 

onMounted(async () => {
  const response = await axios.get('https://fakestoreapi.com/products');
  produtos.value = response.data;
});

const formatPrice = (price) => `R$ ${price.toFixed(2).replace('.', ',')}`;
</script>

<template>
  <div>
    <div v-if="isMobile">É móvel</div>
    <div class="container">
      <div class="card" v-for="produto in produtos" :key="produto.id">
        <div>
        <img class="card--avatar" :src="produto.image" :alt="produto.title" />
      </div>
        <h1 class="card--title">{{ produto.title }}</h1>
        <p>{{ produto.description.slice(0,30)}}</p>
        <p>{{ formatPrice(produto.price) }}</p>
        <button> Adicione ao carrinho </button>
      </div>
    </div>
  </div>

  <Footer v-if="isMobile"/>
</template>

<style scoped>
@media (max-width: 768px) {
  .container {
    gap: 0.5rem;
  }
  .card {
    width: 92%;
  }
}

@media (min-width: 768px) and (max-width: 1024px) {
  .card {
    width: 22rem;
  }
}
.container {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  justify-content: center;
  align-items: center;
  margin: auto;
  padding: 1rem 0;
}
.card {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-direction: column;
  width: 15rem;
  height: 25rem;
  background: #fff;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  border-radius: 5px;
  margin: auto;
  overflow: hidden;
  transition: 0.3s;
}
card:hover{
  box-shadow: 0 8px 18px 0 rgba(0, 0, 0, 0.2);
}
.card--avatar {
  width: 10rem;
  height: 15rem;
  object-fit: cover;
  margin-bottom: 0.5rem;
}
.card--title {
  color: #222;
  font-weight: 500;
  text-transform: capitalize;
  font-size: 0.8rem;
  margin-top: 0.5rem;
margin: 10px;
}
.card button {
  border:none;
  outline:0;
  padding: 8px;
  color: rgb(255, 255, 255);
  background-color:rgb(216, 52, 93);
  text-align: center;
  cursor:pointer;
  border-radius: 5px;
  margin-top: 10px;
  margin-bottom: 10px;
}
</style>