<template>
  <div id="app" class="min-h-screen">
    <!-- Header Component -->
    <HeaderComponent 
      :cart-count="cartCount" 
      @toggle-cart="toggleCart" 
    />
    
    <!-- Hero Section com Carousel -->
    <HeroSection :featured-products="featuredProducts" />
    
    <!-- Produtos -->
    <ProductSection 
      :products="products" 
      @add-to-cart="addToCart" 
    />
    
    <!-- Footer Component -->
    <FooterComponent />
    
    <!-- Cart Sidebar -->
    <CartSidebar 
      v-if="showCart" 
      :cart-items="cartItems" 
      @close="toggleCart"
      @update-quantity="updateCartQuantity"
      @remove-item="removeFromCart"
    />
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import HeaderComponent from './components/HeaderComponent.vue'
import HeroSection from './components/HeroSection.vue'
import ProductSection from './components/ProductSection.vue'
import FooterComponent from './components/FooterComponent.vue'
import CartSidebar from './components/CartSidebar.vue'

// Estado reativo
const cartItems = ref([])
const showCart = ref(false)

// Produtos mock (substitua pelas suas imagens)
const products = ref([
  {
    id: 1,
    name: 'Vaso Grego Clássico',
    price: 299.90,
    image: '/imagens/vaso1.jpg',
    description: 'Réplica autêntica de vaso da Grécia Antiga',
    category: 'Decoração',
    featured: true
  },
  {
    id: 2,
    name: 'Estátua de Atena',
    price: 599.90,
    image: '/imagens/estatua1.jpg',
    description: 'Majestosa estátua da deusa da sabedoria',
    category: 'Esculturas',
    featured: true
  },
  {
    id: 3,
    name: 'Moeda Antiga Reprodução',
    price: 99.90,
    image: '/imagens/moeda1.jpg',
    description: 'Reprodução de moeda grega antiga',
    category: 'Colecionáveis',
    featured: false
  },
  {
    id: 4,
    name: 'Prato Decorativo Grego',
    price: 189.90,
    image: '/imagens/prato1.jpg',
    description: 'Prato com motivos gregos tradicionais',
    category: 'Decoração',
    featured: true
  },
  {
    id: 5,
    name: 'Busto de Sócrates',
    price: 399.90,
    image: '/imagens/busto1.jpg',
    description: 'Busto do grande filósofo grego',
    category: 'Esculturas',
    featured: false
  },
  {
    id: 6,
    name: 'Ânfora Decorativa',
    price: 449.90,
    image: '/imagens/anfora1.jpg',
    description: 'Ânfora grega com pinturas originais',
    category: 'Decoração',
    featured: true
  }
])

// Computed
const cartCount = computed(() => {
  return cartItems.value.reduce((total, item) => total + item.quantity, 0)
})

const featuredProducts = computed(() => {
  return products.value.filter(product => product.featured)
})

// Métodos
const toggleCart = () => {
  showCart.value = !showCart.value
}

const addToCart = (product) => {
  const existingItem = cartItems.value.find(item => item.id === product.id)
  
  if (existingItem) {
    existingItem.quantity += 1
  } else {
    cartItems.value.push({
      ...product,
      quantity: 1
    })
  }
}

const updateCartQuantity = (productId, newQuantity) => {
  const item = cartItems.value.find(item => item.id === productId)
  if (item) {
    item.quantity = newQuantity
  }
}

const removeFromCart = (productId) => {
  const index = cartItems.value.findIndex(item => item.id === productId)
  if (index > -1) {
    cartItems.value.splice(index, 1)
  }
}
</script>