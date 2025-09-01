<template>
  <header class="relative">
    <!-- Top Bar -->
    <div class="bg-dark-blue text-white py-2 text-sm">
      <div class="container mx-auto px-4">
        <div class="flex items-center justify-between">
          <div class="flex items-center space-x-6">
            <span class="flex items-center">
              <i class="fas fa-truck mr-2 text-gold"></i>
              Frete grátis acima de R$ 200
            </span>
            <span class="hidden md:flex items-center">
              <i class="fas fa-shield-alt mr-2 text-gold"></i>
              Compra 100% segura
            </span>
          </div>
          <div class="flex items-center space-x-4">
            <a href="tel:+5511999999999" class="hover:text-gold transition-colors">
              <i class="fas fa-phone mr-1"></i>
              (11) 99999-9999
            </a>
            <div class="flex space-x-2">
              <a href="#" class="text-gray-300 hover:text-gold transition-colors">
                <i class="fab fa-facebook-f"></i>
              </a>
              <a href="#" class="text-gray-300 hover:text-gold transition-colors">
                <i class="fab fa-instagram"></i>
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Main Header -->
    <div class="bg-white shadow-lg sticky top-0 z-50 backdrop-blur-sm bg-white/95">
      <div class="container mx-auto px-4">
        <div class="flex items-center justify-between py-4">
          
          <!-- Logo -->
          <div class="flex items-center space-x-3 group cursor-pointer" @click="scrollToTop">
            <div class="relative">
              <div class="text-4xl group-hover:scale-110 transition-transform duration-300">🏛️</div>
              <div class="absolute -inset-2 bg-gold/20 rounded-full opacity-0 group-hover:opacity-100 transition-opacity duration-300"></div>
            </div>
            <div>
              <h1 class="text-2xl md:text-3xl font-bold text-gold-gradient font-cinzel">
                HELLENIC
              </h1>
              <p class="text-xs md:text-sm text-gray-600 font-cinzel">
                Experiência da Grécia Antiga
              </p>
            </div>
          </div>

          <!-- Desktop Navigation -->
          <nav class="hidden lg:flex items-center space-x-8">
            <a 
              v-for="item in navigation" 
              :key="item.name"
              :href="item.href" 
              class="relative px-3 py-2 text-gray-700 hover:text-gold transition-all duration-300 font-cinzel font-medium group"
              @click="handleNavClick($event, item.href)"
            >
              {{ item.name }}
              <span class="absolute bottom-0 left-0 w-full h-0.5 bg-gold transform scale-x-0 group-hover:scale-x-100 transition-transform duration-300 origin-left"></span>
            </a>
          </nav>

          <!-- Right Side Actions -->
          <div class="flex items-center space-x-4">
            
            <!-- Search (Desktop) -->
            <div class="hidden lg:flex items-center relative">
              <div class="relative">
                <input 
                  type="text" 
                  placeholder="Buscar produtos..." 
                  class="w-64 px-4 py-2 pr-10 border border-gray-300 rounded-full focus:outline-none focus:border-gold focus:ring-2 focus:ring-gold/20 transition-all duration-300"
                  v-model="searchQuery"
                  @input="handleSearch"
                  @focus="showSearchSuggestions = true"
                  @blur="hideSearchSuggestions"
                >
                <button class="absolute right-2 top-1/2 transform -translate-y-1/2 text-gray-400 hover:text-gold transition-colors">
                  <i class="fas fa-search"></i>
                </button>
              </div>
              
              <!-- Search Suggestions -->
              <div 
                v-if="showSearchSuggestions && searchSuggestions.length > 0"
                class="absolute top-full left-0 right-0 mt-2 bg-white border border-gray-200 rounded-lg shadow-xl z-50 max-h-64 overflow-y-auto"
              >
                <div 
                  v-for="suggestion in searchSuggestions" 
                  :key="suggestion"
                  class="px-4 py-2 hover:bg-gray-50 cursor-pointer text-sm text-gray-700 hover:text-gold transition-colors"
                  @mousedown="selectSuggestion(suggestion)"
                >
                  <i class="fas fa-search mr-2 text-gray-400"></i>
                  {{ suggestion }}
                </div>
              </div>
            </div>

            <!-- Wishlist -->
            <button 
              class="relative p-2 text-gray-700 hover:text-gold transition-all duration-300 hover:bg-gold/5 rounded-full"
              title="Lista de desejos"
            >
              <i class="fas fa-heart text-xl"></i>
              <span class="absolute -top-1 -right-1 bg-gold text-white text-xs rounded-full h-5 w-5 flex items-center justify-center font-bold text-[10px]">
                0
              </span>
            </button>

            <!-- User Account -->
            <div class="relative">
              <button 
                @click="toggleUserMenu"
                class="flex items-center space-x-2 p-2 text-gray-700 hover:text-gold transition-all duration-300 hover:bg-gold/5 rounded-full"
              >
                <i class="fas fa-user text-xl"></i>
                <i class="fas fa-chevron-down text-xs hidden md:block"></i>
              </button>
              
              <!-- User Dropdown -->
              <div 
                v-if="showUserMenu"
                class="absolute right-0 top-full mt-2 w-48 bg-white border border-gray-200 rounded-lg shadow-xl z-50"
              >
                <div class="py-2">
                  <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-50 hover:text-gold transition-colors">
                    <i class="fas fa-sign-in-alt mr-2"></i>
                    Entrar
                  </a>
                  <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-50 hover:text-gold transition-colors">
                    <i class="fas fa-user-plus mr-2"></i>
                    Cadastrar
                  </a>
                  <hr class="my-1 border-gray-200">
                  <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-50 hover:text-gold transition-colors">
                    <i class="fas fa-box mr-2"></i>
                    Meus Pedidos
                  </a>
                  <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-50 hover:text-gold transition-colors">
                    <i class="fas fa-user-cog mr-2"></i>
                    Minha Conta
                  </a>
                </div>
              </div>
            </div>

            <!-- Cart Button -->
            <button 
              @click="$emit('toggle-cart')"
              class="relative p-3 text-gray-700 hover:text-gold transition-all duration-300 hover:bg-gold/5 rounded-full group"
            >
              <i class="fas fa-shopping-cart text-xl group-hover:scale-110 transition-transform"></i>
              <span 
                v-if="cartCount > 0"
                class="absolute -top-1 -right-1 bg-gold text-white text-xs rounded-full h-6 w-6 flex items-center justify-center font-bold animate-pulse"
              >
                {{ cartCount }}
              </span>
            </button>

            <!-- Mobile Menu Toggle -->
            <button 
              @click="toggleMobileMenu"
              class="lg:hidden p-2 text-gray-700 hover:text-gold transition-all duration-300 hover:bg-gold/5 rounded-full"
            >
              <i :class="showMobileMenu ? 'fas fa-times' : 'fas fa-bars'" class="text-xl transition-transform duration-300"></i>
            </button>
          </div>
        </div>

        <!-- Mobile Menu -->
        <div 
          :class="[
            'lg:hidden overflow-hidden transition-all duration-300',
            showMobileMenu ? 'max-h-96 pb-4' : 'max-h-0'
          ]"
        >
          <div class="border-t border-gray-200 pt-4">
            <!-- Mobile Search -->
            <div class="mb-4">
              <div class="relative">
                <input 
                  type="text" 
                  placeholder="Buscar produtos..." 
                  class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:border-gold focus:ring-2 focus:ring-gold/20"
                  v-model="searchQuery"
                >
                <button class="absolute right-3 top-1/2 transform -translate-y-1/2 text-gray-400">
                  <i class="fas fa-search"></i>
                </button>
              </div>
            </div>

            <!-- Mobile Navigation -->
            <nav class="space-y-1">
              <a 
                v-for="item in navigation" 
                :key="item.name"
                :href="item.href" 
                class="block py-3 px-4 text-gray-700 hover:text-gold hover:bg-gray-50 rounded-lg transition-all duration-300 font-cinzel font-medium"
                @click="handleMobileNavClick($event, item.href)"
              >
                <i :class="item.icon" class="mr-3 w-5"></i>
                {{ item.name }}
              </a>
            </nav>

            <!-- Mobile Quick Actions -->
            <div class="mt-4 pt-4 border-t border-gray-200 grid grid-cols-2 gap-3">
              <button class="flex items-center justify-center py-2 px-4 bg-gold/10 text-gold rounded-lg hover:bg-gold/20 transition-colors">
                <i class="fas fa-heart mr-2"></i>
                Favoritos
              </button>
              <button class="flex items-center justify-center py-2 px-4 bg-gold/10 text-gold rounded-lg hover:bg-gold/20 transition-colors">
                <i class="fas fa-user mr-2"></i>
                Conta
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </header>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

// Props
defineProps({
  cartCount: {
    type: Number,
    default: 0
  }
})

// Emits
defineEmits(['toggle-cart'])

// Estado reativo
const showMobileMenu = ref(false)
const showUserMenu = ref(false)
const showSearchSuggestions = ref(false)
const searchQuery = ref('')

// Navegação com ícones
const navigation = [
  { name: 'Início', href: '#home', icon: 'fas fa-home' },
  { name: 'Produtos', href: '#products', icon: 'fas fa-shopping-bag' },
  { name: 'Categorias', href: '#categories', icon: 'fas fa-th-large' },
  { name: 'Sobre', href: '#about', icon: 'fas fa-info-circle' },
  { name: 'Contato', href: '#contact', icon: 'fas fa-envelope' }
]

// Sugestões de busca
const searchSuggestions = computed(() => {
  if (!searchQuery.value) return []
  
  const suggestions = [
    'Vaso grego',
    'Estátua de Atena',
    'Moedas antigas',
    'Pratos decorativos',
    'Bustos gregos',
    'Ânforas',
    'Arte grega',
    'Decoração clássica'
  ]
  
  return suggestions.filter(suggestion => 
    suggestion.toLowerCase().includes(searchQuery.value.toLowerCase())
  ).slice(0, 5)
})

// Métodos
const toggleMobileMenu = () => {
  showMobileMenu.value = !showMobileMenu.value
  showUserMenu.value = false // Fechar menu do usuário
}

const toggleUserMenu = () => {
  showUserMenu.value = !showUserMenu.value
  showMobileMenu.value = false // Fechar menu mobile
}

const scrollToTop = () => {
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

const handleNavClick = (event, href) => {
  if (href.startsWith('#')) {
    event.preventDefault()
    const element = document.querySelector(href)
    if (element) {
      element.scrollIntoView({ behavior: 'smooth' })
    }
  }
}

const handleMobileNavClick = (event, href) => {
  handleNavClick(event, href)
  showMobileMenu.value = false
}

const handleSearch = () => {
  // Implementar lógica de busca
  console.log('Searching for:', searchQuery.value)
}

const selectSuggestion = (suggestion) => {
  searchQuery.value = suggestion
  showSearchSuggestions.value = false
  handleSearch()
}

const hideSearchSuggestions = () => {
  // Delay para permitir clique nas sugestões
  setTimeout(() => {
    showSearchSuggestions.value = false
  }, 200)
}

// Fechar menus quando clicar fora
const handleClickOutside = (event) => {
  const userMenu = event.target.closest('.relative')
  if (!userMenu) {
    showUserMenu.value = false
  }
}

onMounted(() => {
  document.addEventListener('click', handleClickOutside)
})

onUnmounted(() => {
  document.removeEventListener('click', handleClickOutside)
})
</script>