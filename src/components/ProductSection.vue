<template>
  <section id="products" class="section-padding bg-gradient-to-br from-white to-gray-50">
    <div class="container mx-auto px-4">
      
      <!-- Header da seção -->
      <div class="text-center mb-16 animate-fade-in-up">
        <div class="decorative-divider">
          <div class="icon">
            <i class="fas fa-gems"></i>
          </div>
        </div>
        
        <h2 class="text-4xl md:text-5xl lg:text-6xl font-bold text-dark-blue font-cinzel mb-6">
          Nossa <span class="text-gold-gradient">Coleção</span>
        </h2>
        
        <p class="text-lg md:text-xl text-gray-600 font-inter max-w-3xl mx-auto leading-relaxed">
          Artefatos cuidadosamente selecionados que trazem a essência e majestade da Grécia Antiga para transformar seu ambiente
        </p>
      </div>

      <!-- Controles superiores -->
      <div class="flex flex-col lg:flex-row justify-between items-center gap-6 mb-12">
        
        <!-- Filtros de categoria -->
        <div class="flex flex-wrap justify-center gap-3">
          <button 
            v-for="category in categories"
            :key="category"
            @click="setFilter(category)"
            :class="[
              'px-6 py-3 font-semibold font-cinzel transition-all duration-300 rounded-full relative overflow-hidden group',
              selectedCategory === category 
                ? 'bg-gold text-white shadow-lg transform scale-105' 
                : 'bg-white text-gray-700 hover:bg-gray-50 border-2 border-gray-200 hover:border-gold/50'
            ]"
          >
            <span class="relative z-10">{{ category }}</span>
            <div 
              v-if="selectedCategory !== category"
              class="absolute inset-0 bg-gradient-to-r from-gold to-dark-gold transform translate-x-full group-hover:translate-x-0 transition-transform duration-300"
            ></div>
          </button>
        </div>

        <!-- Controles de visualização e ordenação -->
        <div class="flex items-center gap-4">
          <!-- Ordenação -->
          <div class="relative">
            <select 
              v-model="sortBy"
              class="appearance-none bg-white border-2 border-gray-200 rounded-lg px-4 py-2 pr-8 text-gray-700 focus:outline-none focus:border-gold transition-colors"
            >
              <option value="default">Ordenar por</option>
              <option value="price-low">Menor preço</option>
              <option value="price-high">Maior preço</option>
              <option value="name">Nome A-Z</option>
              <option value="featured">Destaques</option>
            </select>
            <i class="fas fa-chevron-down absolute right-3 top-1/2 transform -translate-y-1/2 text-gray-400 pointer-events-none"></i>
          </div>

          <!-- Visualização -->
          <div class="flex bg-gray-100 rounded-lg p-1">
            <button 
              @click="viewMode = 'grid'"
              :class="[
                'p-2 rounded transition-colors',
                viewMode === 'grid' ? 'bg-white shadow-sm text-gold' : 'text-gray-500 hover:text-gray-700'
              ]"
            >
              <i class="fas fa-th-large"></i>
            </button>
            <button 
              @click="viewMode = 'list'"
              :class="[
                'p-2 rounded transition-colors',
                viewMode === 'list' ? 'bg-white shadow-sm text-gold' : 'text-gray-500 hover:text-gray-700'
              ]"
            >
              <i class="fas fa-list"></i>
            </button>
          </div>
        </div>
      </div>

      <!-- Resultados da busca -->
      <div v-if="selectedCategory !== 'Todos'" class="mb-8 text-center">
        <p class="text-gray-600">
          Exibindo <strong class="text-gold">{{ filteredProducts.length }}</strong> 
          produto{{ filteredProducts.length !== 1 ? 's' : '' }} 
          em <strong>{{ selectedCategory }}</strong>
        </p>
      </div>

      <!-- Grid de Produtos -->
      <div 
        :class="[
          'grid gap-8 mb-12',
          viewMode === 'grid' 
            ? 'grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4' 
            : 'grid-cols-1 max-w-4xl mx-auto'
        ]"
      >
        <div 
          v-for="(product, index) in paginatedProducts" 
          :key="product.id"
          :class="[
            'card-greek group relative',
            viewMode === 'list' ? 'flex flex-col md:flex-row' : ''
          ]"
          :style="{ animationDelay: `${index * 0.1}s` }"
          class="animate-fade-in-up"
        >
          <!-- Imagem do Produto -->
          <div 
            :class="[
              'relative overflow-hidden',
              viewMode === 'list' ? 'md:w-64 md:flex-shrink-0' : 'aspect-square'
            ]"
          >
            <img 
              :src="product.image" 
              :alt="product.name"
              :class="[
                'w-full object-cover group-hover:scale-110 transition-transform duration-500',
                viewMode === 'list' ? 'h-48 md:h-full' : 'h-full'
              ]"
              @error="handleImageError"
            >
            
            <!-- Badges -->
            <div class="absolute top-4 left-4 flex flex-col gap-2">
              <div 
                v-if="product.featured"
                class="bg-gold text-white px-3 py-1 text-xs font-bold font-cinzel rounded-full shadow-md"
              >
                <i class="fas fa-star mr-1"></i>
                DESTAQUE
              </div>
              
              <div 
                v-if="product.discount"
                class="bg-red-500 text-white px-3 py-1 text-xs font-bold rounded-full shadow-md"
              >
                -{{ product.discount }}%
              </div>
            </div>

            <!-- Overlay com ações rápidas -->
            <div class="absolute inset-0 bg-black/50 opacity-0 group-hover:opacity-100 transition-all duration-300 flex items-center justify-center">
              <div class="flex gap-3 transform translate-y-4 group-hover:translate-y-0 transition-transform duration-300">
                <button 
                  @click="quickView(product)"
                  class="w-12 h-12 bg-white/90 hover:bg-white text-gray-800 rounded-full flex items-center justify-center hover:scale-110 transition-all duration-300 shadow-lg"
                  title="Visualização rápida"
                >
                  <i class="fas fa-eye"></i>
                </button>
                
                <button 
                  @click="toggleWishlist(product)"
                  :class="[
                    'w-12 h-12 rounded-full flex items-center justify-center hover:scale-110 transition-all duration-300 shadow-lg',
                    isInWishlist(product.id) 
                      ? 'bg-red-500 text-white' 
                      : 'bg-white/90 hover:bg-white text-gray-800'
                  ]"
                  title="Adicionar aos favoritos"
                >
                  <i :class="isInWishlist(product.id) ? 'fas fa-heart' : 'far fa-heart'"></i>
                </button>
                
                <button 
                  @click="shareProduct(product)"
                  class="w-12 h-12 bg-white/90 hover:bg-white text-gray-800 rounded-full flex items-center justify-center hover:scale-110 transition-all duration-300 shadow-lg"
                  title="Compartilhar"
                >
                  <i class="fas fa-share-alt"></i>
                </button>
              </div>
            </div>

            <!-- Indicador de estoque -->
            <div 
              v-if="product.stock <= 5"
              class="absolute bottom-4 left-4 bg-orange-500 text-white px-3 py-1 text-xs font-bold rounded-full"
            >
              Restam {{ product.stock }}
            </div>
          </div>

          <!-- Informações do Produto -->
          <div :class="['p-6', viewMode === 'list' ? 'flex-1' : '']">
            
            <!-- Header do produto -->
            <div class="flex items-start justify-between mb-3">
              <div class="flex-1">
                <span class="inline-block px-2 py-1 bg-gold/10 text-gold text-xs font-medium font-cinzel rounded-full mb-2">
                  {{ product.category }}
                </span>
                
                <h3 class="text-lg md:text-xl font-bold text-gray-800 font-cinzel group-hover:text-gold transition-colors leading-tight">
                  {{ product.name }}
                </h3>
              </div>
              
              <!-- Rating -->
              <div class="flex items-center ml-4">
                <div class="flex text-gold text-sm">
                  <i v-for="n in 5" :key="n" :class="n <= (product.rating || 5) ? 'fas fa-star' : 'far fa-star'"></i>
                </div>
                <span class="text-xs text-gray-500 ml-1">({{ product.reviews || 0 }})</span>
              </div>
            </div>

            <!-- Descrição -->
            <p class="text-gray-600 mb-4 text-sm leading-relaxed line-clamp-2">
              {{ product.description }}
            </p>

            <!-- Características -->
            <div v-if="product.features" class="mb-4">
              <div class="flex flex-wrap gap-2">
                <span 
                  v-for="feature in product.features.slice(0, 3)" 
                  :key="feature"
                  class="text-xs bg-gray-100 text-gray-600 px-2 py-1 rounded-full"
                >
                  {{ feature }}
                </span>
              </div>
            </div>

            <!-- Preço e ações -->
            <div class="flex items-center justify-between mt-auto">
              <div class="flex flex-col">
                <div v-if="product.oldPrice" class="text-sm text-gray-400 line-through">
                  R$ {{ formatPrice(product.oldPrice) }}
                </div>
                <div class="text-xl md:text-2xl font-bold text-gold">
                  R$ {{ formatPrice(product.price) }}
                </div>
                <div v-if="product.installments" class="text-xs text-gray-500">
                  ou {{ product.installments.count }}x de R$ {{ formatPrice(product.installments.value) }}
                </div>
              </div>

              <div class="flex gap-2">
                <!-- Quantidade -->
                <div class="flex items-center bg-gray-100 rounded-lg">
                  <button 
                    @click="updateQuantity(product.id, -1)"
                    class="w-8 h-8 flex items-center justify-center text-gray-600 hover:text-gold transition-colors"
                  >
                    <i class="fas fa-minus text-xs"></i>
                  </button>
                  <span class="w-8 text-center text-sm font-semibold">{{ getQuantity(product.id) }}</span>
                  <button 
                    @click="updateQuantity(product.id, 1)"
                    class="w-8 h-8 flex items-center justify-center text-gray-600 hover:text-gold transition-colors"
                  >
                    <i class="fas fa-plus text-xs"></i>
                  </button>
                </div>

                <!-- Botão adicionar ao carrinho -->
                <button 
                  @click="addToCart(product)"
                  :disabled="product.stock === 0"
                  :class="[
                    'px-4 py-2 font-semibold transition-all duration-300 rounded-lg flex items-center gap-2 group/btn',
                    product.stock === 0
                      ? 'bg-gray-300 text-gray-500 cursor-not-allowed'
                      : 'btn-greek hover:shadow-lg hover:-translate-y-0.5'
                  ]"
                >
                  <i class="fas fa-cart-plus group-hover/btn:animate-pulse"></i>
                  <span class="hidden sm:inline">
                    {{ product.stock === 0 ? 'Esgotado' : 'Adicionar' }}
                  </span>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Paginação -->
      <div v-if="totalPages > 1" class="flex justify-center items-center gap-2">
        <button 
          @click="goToPage(currentPage - 1)"
          :disabled="currentPage === 1"
          class="px-4 py-2 border border-gray-300 rounded-lg disabled:opacity-50 disabled:cursor-not-allowed hover:bg-gray-50 transition-colors"
        >
          <i class="fas fa-chevron-left"></i>
        </button>

        <div class="flex gap-1">
          <button 
            v-for="page in visiblePages"
            :key="page"
            @click="goToPage(page)"
            :class="[
              'w-10 h-10 rounded-lg font-medium transition-colors',
              page === currentPage
                ? 'bg-gold text-white'
                : 'border border-gray-300 hover:bg-gray-50'
            ]"
          >
            {{ page }}
          </button>
        </div>

        <button 
          @click="goToPage(currentPage + 1)"
          :disabled="currentPage === totalPages"
          class="px-4 py-2 border border-gray-300 rounded-lg disabled:opacity-50 disabled:cursor-not-allowed hover:bg-gray-50 transition-colors"
        >
          <i class="fas fa-chevron-right"></i>
        </button>
      </div>

      <!-- Call to Action -->
      <div class="text-center mt-16 p-8 bg-gradient-to-r from-gold/10 to-dark-gold/10 rounded-2xl border border-gold/20">
        <h3 class="text-2xl font-bold text-dark-blue font-cinzel mb-4">
          Não encontrou o que procurava?
        </h3>
        <p class="text-gray-600 mb-6 max-w-2xl mx-auto">
          Nossa equipe de especialistas pode ajudá-lo a encontrar a peça perfeita para sua coleção ou criar algo exclusivo sob medida.
        </p>
        <div class="flex flex-col sm:flex-row gap-4 justify-center">
          <button class="btn-greek px-6 py-3">
            <i class="fas fa-phone mr-2"></i>
            Falar com Especialista
          </button>
          <button class="border-2 border-gold text-gold px-6 py-3 font-semibold hover:bg-gold hover:text-white transition-all duration-300 font-cinzel rounded-lg">
            <i class="fas fa-palette mr-2"></i>
            Peças Sob Medida
          </button>
        </div>
      </div>

      <!-- Modal de Visualização Rápida -->
      <div 
        v-if="showQuickView && selectedProduct"
        class="fixed inset-0 bg-black/60 z-50 flex items-center justify-center p-4 backdrop-blur-sm"
        @click="closeQuickView"
      >
        <div 
          class="bg-white rounded-2xl max-w-6xl w-full max-h-[90vh] overflow-y-auto shadow-2xl"
          @click.stop
        >
          <div class="relative">
            <!-- Botão Fechar -->
            <button 
              @click="closeQuickView"
              class="absolute top-4 right-4 z-10 w-12 h-12 bg-white/90 hover:bg-white text-gray-800 rounded-full flex items-center justify-center shadow-lg hover:scale-110 transition-all duration-300"
            >
              <i class="fas fa-times"></i>
            </button>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 p-8">
              <!-- Galeria de imagens -->
              <div>
                <div class="aspect-square mb-4 rounded-xl overflow-hidden">
                  <img 
                    :src="selectedProduct.image" 
                    :alt="selectedProduct.name"
                    class="w-full h-full object-cover"
                  >
                </div>
                
                <!-- Thumbnails -->
                <div class="grid grid-cols-4 gap-2">
                  <div 
                    v-for="i in 4" 
                    :key="i"
                    class="aspect-square bg-gray-100 rounded-lg overflow-hidden cursor-pointer hover:ring-2 hover:ring-gold transition-all"
                  >
                    <img 
                      :src="selectedProduct.image" 
                      :alt="`${selectedProduct.name} - ${i}`"
                      class="w-full h-full object-cover opacity-80 hover:opacity-100 transition-opacity"
                    >
                  </div>
                </div>
              </div>

              <!-- Detalhes do produto -->
              <div class="flex flex-col">
                <div class="mb-4">
                  <span class="inline-block px-3 py-1 bg-gold/10 text-gold text-sm font-medium font-cinzel rounded-full mb-3">
                    {{ selectedProduct.category }}
                  </span>
                  
                  <h3 class="text-3xl lg:text-4xl font-bold text-gray-800 font-cinzel mb-4">
                    {{ selectedProduct.name }}
                  </h3>

                  <!-- Rating -->
                  <div class="flex items-center gap-4 mb-6">
                    <div class="flex text-gold text-lg">
                      <i v-for="n in 5" :key="n" :class="n <= (selectedProduct.rating || 5) ? 'fas fa-star' : 'far fa-star'"></i>
                    </div>
                    <span class="text-gray-600">{{ selectedProduct.reviews || 0 }} avaliações</span>
                  </div>
                </div>

                <p class="text-gray-600 mb-6 text-lg leading-relaxed">
                  {{ selectedProduct.description }}
                </p>

                <!-- Características detalhadas -->
                <div v-if="selectedProduct.features" class="mb-6">
                  <h4 class="font-semibold text-gray-800 mb-3 font-cinzel">Características:</h4>
                  <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
                    <div 
                      v-for="feature in selectedProduct.features" 
                      :key="feature"
                      class="flex items-center text-sm text-gray-600"
                    >
                      <i class="fas fa-check text-gold mr-2"></i>
                      {{ feature }}
                    </div>
                  </div>
                </div>

                <!-- Preço -->
                <div class="mb-8">
                  <div v-if="selectedProduct.oldPrice" class="text-lg text-gray-400 line-through mb-1">
                    R$ {{ formatPrice(selectedProduct.oldPrice) }}
                  </div>
                  <div class="text-4xl font-bold text-gold mb-2">
                    R$ {{ formatPrice(selectedProduct.price) }}
                  </div>
                  <div v-if="selectedProduct.installments" class="text-gray-600">
                    ou {{ selectedProduct.installments.count }}x de R$ {{ formatPrice(selectedProduct.installments.value) }} sem juros
                  </div>
                </div>

                <!-- Ações -->
                <div class="flex gap-4 mb-6">
                  <!-- Seletor de quantidade -->
                  <div class="flex items-center bg-gray-100 rounded-lg p-1">
                    <button 
                      @click="updateQuantity(selectedProduct.id, -1)"
                      class="w-10 h-10 flex items-center justify-center text-gray-600 hover:text-gold transition-colors rounded"
                    >
                      <i class="fas fa-minus"></i>
                    </button>
                    <span class="w-12 text-center font-semibold">{{ getQuantity(selectedProduct.id) }}</span>
                    <button 
                      @click="updateQuantity(selectedProduct.id, 1)"
                      class="w-10 h-10 flex items-center justify-center text-gray-600 hover:text-gold transition-colors rounded"
                    >
                      <i class="fas fa-plus"></i>
                    </button>
                  </div>

                  <button 
                    @click="addToCart(selectedProduct)"
                    class="btn-greek flex-1 py-4 text-lg"
                    :disabled="selectedProduct.stock === 0"
                  >
                    <i class="fas fa-cart-plus mr-2"></i>
                    {{ selectedProduct.stock === 0 ? 'Produto Esgotado' : 'Adicionar ao Carrinho' }}
                  </button>
                  
                  <button 
                    @click="toggleWishlist(selectedProduct)"
                    :class="[
                      'w-14 h-14 rounded-lg border-2 flex items-center justify-center transition-all duration-300',
                      isInWishlist(selectedProduct.id)
                        ? 'border-red-500 bg-red-500 text-white'
                        : 'border-gold text-gold hover:bg-gold hover:text-white'
                    ]"
                  >
                    <i :class="isInWishlist(selectedProduct.id) ? 'fas fa-heart' : 'far fa-heart'"></i>
                  </button>
                </div>

                <!-- Informações adicionais -->
                <div class="space-y-3 text-sm text-gray-600 pt-6 border-t border-gray-200">
                  <div class="flex items-center">
                    <i class="fas fa-truck text-gold mr-3"></i>
                    <span>Frete grátis para pedidos acima de R$ 200</span>
                  </div>
                  <div class="flex items-center">
                    <i class="fas fa-shield-alt text-gold mr-3"></i>
                    <span>Garantia de 30 dias</span>
                  </div>
                  <div class="flex items-center">
                    <i class="fas fa-medal text-gold mr-3"></i>
                    <span>Produto autenticado e certificado</span>
                  </div>
                  <div class="flex items-center">
                    <i class="fas fa-exchange-alt text-gold mr-3"></i>
                    <span>Troca grátis em até 7 dias</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed } from 'vue'

// Props
const props = defineProps({
  products: {
    type: Array,
    default: () => []
  }
})

// Emits
const emit = defineEmits(['add-to-cart'])

// Estado reativo
const selectedCategory = ref('Todos')
const showQuickView = ref(false)
const selectedProduct = ref(null)
const viewMode = ref('grid')
const sortBy = ref('default')
const currentPage = ref(1)
const itemsPerPage = 12
const quantities = ref({})
const wishlist = ref(new Set())

// Computed
const categories = computed(() => {
  const cats = ['Todos', ...new Set(props.products.map(p => p.category))]
  return cats
})

const filteredProducts = computed(() => {
  let filtered = props.products
  
  if (selectedCategory.value !== 'Todos') {
    filtered = filtered.filter(product => product.category === selectedCategory.value)
  }
  
  // Aplicar ordenação
  switch (sortBy.value) {
    case 'price-low':
      filtered = [...filtered].sort((a, b) => a.price - b.price)
      break
    case 'price-high':
      filtered = [...filtered].sort((a, b) => b.price - a.price)
      break
    case 'name':
      filtered = [...filtered].sort((a, b) => a.name.localeCompare(b.name))
      break
    case 'featured':
      filtered = [...filtered].sort((a, b) => (b.featured ? 1 : 0) - (a.featured ? 1 : 0))
      break
  }
  
  return filtered
})

const totalPages = computed(() => {
  return Math.ceil(filteredProducts.value.length / itemsPerPage)
})

const paginatedProducts = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage
  const end = start + itemsPerPage
  return filteredProducts.value.slice(start, end)
})

const visiblePages = computed(() => {
  const pages = []
  const total = totalPages.value
  const current = currentPage.value
  
  if (total <= 7) {
    for (let i = 1; i <= total; i++) {
      pages.push(i)
    }
  } else {
    if (current <= 4) {
      for (let i = 1; i <= 5; i++) pages.push(i)
      pages.push('...', total)
    } else if (current >= total - 3) {
      pages.push(1, '...')
      for (let i = total - 4; i <= total; i++) pages.push(i)
    } else {
      pages.push(1, '...', current - 1, current, current + 1, '...', total)
    }
  }
  
  return pages.filter(p => p !== '...' || pages.indexOf(p) === pages.lastIndexOf(p))
})

// Métodos
const setFilter = (category) => {
  selectedCategory.value = category
  currentPage.value = 1
}

const formatPrice = (price) => {
  return price.toFixed(2).replace('.', ',')
}

const handleImageError = (event) => {
  event.target.src = "data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 400 300'%3E%3Crect width='400' height='300' fill='%23f3f4f6'/%3E%3Ctext x='200' y='150' text-anchor='middle' font-family='Arial' font-size='18' fill='%236b7280'%3EProduto%3C/text%3E%3C/svg%3E"
}

const quickView = (product) => {
  selectedProduct.value = product
  showQuickView.value = true
  document.body.style.overflow = 'hidden'
}

const closeQuickView = () => {
  showQuickView.value = false
  selectedProduct.value = null
  document.body.style.overflow = 'auto'
}

const getQuantity = (productId) => {
  return quantities.value[productId] || 1
}

const updateQuantity = (productId, change) => {
  const current = getQuantity(productId)
  const newQuantity = Math.max(1, current + change)
  quantities.value[productId] = newQuantity
}

const addToCart = (product) => {
  const quantity = getQuantity(product.id)
  for (let i = 0; i < quantity; i++) {
    emit('add-to-cart', product)
  }
  // Reset quantity after adding
  quantities.value[product.id] = 1
}

const isInWishlist = (productId) => {
  return wishlist.value.has(productId)
}

const toggleWishlist = (product) => {
  if (wishlist.value.has(product.id)) {
    wishlist.value.delete(product.id)
  } else {
    wishlist.value.add(product.id)
  }
}

const shareProduct = (product) => {
  if (navigator.share) {
    navigator.share({
      title: product.name,
      text: product.description,
      url: window.location.href
    })
  } else {
    // Fallback para cópia do link
    navigator.clipboard.writeText(window.location.href)
    // Aqui você pode mostrar uma notificação de sucesso
    console.log('Link copiado!')
  }
}

const goToPage = (page) => {
  if (page >= 1 && page <= totalPages.value) {
    currentPage.value = page
    // Scroll para o topo da seção de produtos
    document.querySelector('#products').scrollIntoView({ behavior: 'smooth' })
  }
}
</script>