<template>
  <section class="relative flex items-center min-h-screen overflow-hidden">
    <!-- Background com gradiente e padrões -->
    <div class="absolute inset-0">
      <div class="absolute inset-0 bg-gradient-to-br from-cream via-white to-dark-cream"></div>
      <div class="absolute inset-0 greek-pattern opacity-5"></div>
      <div class="absolute inset-0 bg-gradient-to-t from-white/50 via-transparent to-transparent"></div>
    </div>

    <!-- Partículas flutuantes decorativas -->
    <div class="absolute inset-0">
      <div 
        v-for="i in 12" 
        :key="i"
        class="absolute w-2 h-2 rounded-full bg-gold/20 animate-pulse"
        :style="getParticleStyle(i)"
      ></div>
    </div>
    
    <div class="container relative z-10 px-4 mx-auto">
      <div class="grid items-center min-h-screen grid-cols-1 gap-12 py-20 lg:grid-cols-2">
        
        <!-- Conteúdo principal -->
        <div class="text-center lg:text-left animate-fade-in-left">
          <!-- Badge superior -->
          <div class="inline-flex items-center px-4 py-2 mb-8 font-medium border rounded-full bg-gold/10 border-gold/30 text-gold">
            <i class="mr-2 fas fa-crown"></i>
            Coleção Premium 2024
          </div>

          <h1 class="mb-6 text-4xl font-bold leading-tight md:text-5xl lg:text-6xl xl:text-7xl text-dark-blue font-cinzel">
            <span class="block">Majestade da</span>
            <span class="block text-gold-gradient">Grécia Antiga</span>
          </h1>
          
          <p class="max-w-2xl mx-auto mb-8 text-lg leading-relaxed text-gray-600 md:text-xl lg:text-2xl font-inter lg:mx-0">
            Descubra artefatos únicos que trazem a sabedoria e beleza eterna da civilização grega para transformar sua casa em um templo de cultura e elegância
          </p>
          
          <!-- Estatísticas -->
          <div class="grid max-w-md grid-cols-3 gap-6 mx-auto mb-10 lg:mx-0">
            <div class="text-center">
              <div class="text-2xl font-bold text-gold font-cinzel">500+</div>
              <div class="text-sm text-gray-600">Produtos</div>
            </div>
            <div class="text-center">
              <div class="text-2xl font-bold text-gold font-cinzel">50k+</div>
              <div class="text-sm text-gray-600">Clientes</div>
            </div>
            <div class="text-center">
              <div class="text-2xl font-bold text-gold font-cinzel">4.9</div>
              <div class="text-sm text-gray-600">Avaliação</div>
            </div>
          </div>
          
          <!-- Botões de ação -->
          <div class="flex flex-col justify-center gap-4 mb-8 sm:flex-row lg:justify-start">
            <button 
              class="px-8 py-4 text-lg btn-greek group"
              @click="scrollToProducts"
            >
              <i class="mr-2 fas fa-shopping-bag group-hover:animate-pulse"></i>
              Explorar Coleção
            </button>
            <button 
              class="flex items-center justify-center px-8 py-4 font-semibold transition-all duration-300 border-2 rounded-lg border-gold text-gold hover:bg-gold hover:text-white font-cinzel group"
              @click="openVideoModal"
            >
              <div class="flex items-center justify-center w-8 h-8 mr-3 transition-colors rounded-full bg-gold/20 group-hover:bg-white/20">
                <i class="text-sm fas fa-play"></i>
              </div>
              Ver História
            </button>
          </div>

          <!-- Social Proof -->
          <div class="flex items-center justify-center space-x-2 text-sm text-gray-600 lg:justify-start">
            <div class="flex -space-x-2">
              <img 
                src="/imagens/cliente1.png" 
                alt="Cliente satisfeito"
                class="object-cover w-8 h-8 border-2 border-white rounded-full"
                @error="handleAvatarError"
              >
              <img 
                src="/imagens/cliente2.png" 
                alt="Cliente satisfeito"
                class="object-cover w-8 h-8 border-2 border-white rounded-full"
                @error="handleAvatarError"
              >
              <img 
                src="/imagens/cliente3.png" 
                alt="Cliente satisfeito"
                class="object-cover w-8 h-8 border-2 border-white rounded-full"
                @error="handleAvatarError"
              >
              <div class="flex items-center justify-center w-8 h-8 text-xs font-semibold text-gray-600 bg-gray-200 border-2 border-white rounded-full">+1k</div>
            </div>
            <span>Mais de 1000 clientes satisfeitos</span>
          </div>
        </div>

        <!-- Carousel de Produtos -->
        <div class="relative animate-fade-in-right">
          <div class="relative">
            <!-- Carousel Container -->
            <div class="relative overflow-hidden bg-white shadow-2xl rounded-2xl">
              <div 
                class="flex transition-transform duration-700 ease-in-out"
                :style="{ transform: `translateX(-${currentSlide * 100}%)` }"
              >
                <div 
                  v-for="(product, index) in featuredProducts" 
                  :key="product.id"
                  class="relative flex-shrink-0 w-full group"
                >
                  <div class="relative overflow-hidden aspect-square">
                    <img 
                      :src="product.image" 
                      :alt="product.name"
                      class="object-cover w-full h-full transition-transform duration-700 group-hover:scale-110"
                      @error="handleImageError"
                    >
                    
                    <!-- Gradient Overlay -->
                    <div class="absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent"></div>
                    
                    <!-- Product Info -->
                    <div class="absolute bottom-0 left-0 right-0 p-6 text-white">
                      <div class="flex items-center justify-between mb-3">
                        <span class="px-3 py-1 text-xs font-semibold rounded-full bg-gold">
                          {{ product.category }}
                        </span>
                        <div class="flex text-sm text-gold">
                          <i v-for="n in 5" :key="n" class="fas fa-star"></i>
                        </div>
                      </div>
                      
                      <h3 class="mb-2 text-xl font-bold md:text-2xl font-cinzel">
                        {{ product.name }}
                      </h3>
                      
                      <p class="mb-4 text-sm text-gray-200 line-clamp-2">
                        {{ product.description }}
                      </p>
                      
                      <div class="flex items-center justify-between">
                        <div class="text-2xl font-bold md:text-3xl text-gold">
                          R$ {{ formatPrice(product.price) }}
                        </div>
                        
                        <div class="flex space-x-2">
                          <button 
                            class="p-3 transition-all duration-300 rounded-full bg-white/20 hover:bg-white/30 hover:scale-110 backdrop-blur-sm"
                            @click="quickView(product)"
                          >
                            <i class="text-white fas fa-eye"></i>
                          </button>
                          <button 
                            class="px-4 py-3 font-semibold text-white transition-all duration-300 rounded-full shadow-lg bg-gold hover:bg-dark-gold hover:scale-105"
                            @click="$emit('add-to-cart', product)"
                          >
                            <i class="mr-2 fas fa-cart-plus"></i>
                            Adicionar
                          </button>
                        </div>
                      </div>
                    </div>

                    <!-- Slide Indicator -->
                    <div class="absolute px-3 py-1 text-sm text-white rounded-full top-4 right-4 bg-black/50 backdrop-blur-sm">
                      {{ index + 1 }} / {{ featuredProducts.length }}
                    </div>
                  </div>
                </div>
              </div>

              <!-- Navigation Arrows -->
              <button 
                @click="prevSlide"
                class="absolute w-12 h-12 text-gray-800 transition-all duration-300 transform -translate-y-1/2 rounded-full shadow-lg left-4 top-1/2 bg-white/90 hover:bg-white hover:scale-110 backdrop-blur-sm"
              >
                <i class="fas fa-chevron-left"></i>
              </button>
              
              <button 
                @click="nextSlide"
                class="absolute w-12 h-12 text-gray-800 transition-all duration-300 transform -translate-y-1/2 rounded-full shadow-lg right-4 top-1/2 bg-white/90 hover:bg-white hover:scale-110 backdrop-blur-sm"
              >
                <i class="fas fa-chevron-right"></i>
              </button>
            </div>

            <!-- Carousel Dots -->
            <div class="flex justify-center mt-6 space-x-3">
              <button 
                v-for="(_, index) in featuredProducts" 
                :key="index"
                @click="goToSlide(index)"
                :class="[
                  'w-3 h-3 rounded-full transition-all duration-300',
                  currentSlide === index 
                    ? 'bg-gold scale-125 shadow-md' 
                    : 'bg-gray-300 hover:bg-gray-400 hover:scale-110'
                ]"
              ></button>
            </div>

            <!-- Progress Bar -->
            <div class="w-full h-1 mt-4 bg-gray-200 rounded-full">
              <div 
                class="h-1 transition-all duration-300 rounded-full bg-gold"
                :style="{ width: `${((currentSlide + 1) / featuredProducts.length) * 100}%` }"
              ></div>
            </div>
          </div>

          <!-- Decorative Elements -->
          <div class="absolute w-24 h-24 rounded-full -top-4 -right-4 bg-gold/10 blur-xl"></div>
          <div class="absolute w-32 h-32 rounded-full -bottom-8 -left-8 bg-dark-blue/5 blur-2xl"></div>
        </div>
      </div>
    </div>

    <!-- Scroll Indicator -->
    <div class="absolute transform -translate-x-1/2 bottom-8 left-1/2 animate-bounce">
      <div class="flex flex-col items-center text-gray-400">
        <span class="mb-2 text-sm font-cinzel">Explore mais</span>
        <i class="text-xl fas fa-chevron-down"></i>
      </div>
    </div>

    <!-- Decorative wave -->
    <div class="absolute bottom-0 left-0 w-full">
      <svg viewBox="0 0 1200 120" fill="none" xmlns="http://www.w3.org/2000/svg" class="w-full h-20">
        <path 
          d="M0 120L50 110C100 100 200 80 300 70C400 60 500 60 600 65C700 70 800 80 900 85C1000 90 1100 90 1150 90L1200 90V120H0Z" 
          fill="white"
        />
      </svg>
    </div>

    <!-- Video Modal -->
    <Transition
      enter-active-class="duration-300 ease-out"
      enter-from-class="opacity-0"
      enter-to-class="opacity-100"
      leave-active-class="duration-200 ease-in"
      leave-from-class="opacity-100"
      leave-to-class="opacity-0"
    >
      <div 
        v-if="showVideoModal"
        class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/90"
        @click="closeVideoModal"
      >
        <div class="relative w-full max-w-4xl overflow-hidden bg-black rounded-lg shadow-2xl aspect-video">
          <!-- Botão de fechar -->
          <button 
            @click="closeVideoModal"
            class="absolute z-20 flex items-center justify-center w-12 h-12 text-white transition-all duration-300 rounded-full top-4 right-4 bg-black/50 hover:bg-black/70 backdrop-blur-sm hover:scale-110"
          >
            <i class="text-xl fas fa-times"></i>
          </button>
          
          <!-- Loading state -->
          <div 
            v-if="videoLoading" 
            class="absolute inset-0 z-10 flex items-center justify-center text-white"
          >
            <div class="text-center">
              <div class="w-16 h-16 mx-auto mb-4 border-b-2 rounded-full animate-spin border-gold"></div>
              <p class="text-lg font-cinzel">Carregando vídeo...</p>
            </div>
          </div>

          <!-- Video Player -->
          <video 
            ref="videoPlayer"
            class="object-cover w-full h-full"
            controls
            preload="metadata"
            @loadstart="videoLoading = true"
            @canplay="videoLoading = false"
            @error="handleVideoError"
            @click.stop
          >
            <source src="/videos/hellenic.mp4" type="video/mp4">
            <p class="p-4 text-white">
              Seu navegador não suporta vídeos HTML5. 
              <a href="/videos/hellenic.mp4" class="underline text-gold">Clique aqui para baixar o vídeo.</a>
            </p>
          </video>

          <!-- Error state -->
          <div 
            v-if="videoError" 
            class="absolute inset-0 flex items-center justify-center text-white bg-black/80"
          >
            <div class="p-8 text-center">
              <i class="mb-4 text-6xl fas fa-exclamation-triangle text-gold"></i>
              <h3 class="mb-2 text-xl font-cinzel">Erro ao carregar vídeo</h3>
              <p class="mb-4 text-sm text-gray-300">
                Não foi possível reproduzir o vídeo. Verifique se o arquivo existe em /public/videos/hellenic.mp4
              </p>
              <button 
                @click="retryVideo"
                class="px-6 py-3 font-semibold text-white transition-all duration-300 rounded-lg bg-gold hover:bg-dark-gold"
              >
                <i class="mr-2 fas fa-redo"></i>
                Tentar novamente
              </button>
            </div>
          </div>
          
        </div>
      </div>
    </Transition>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue'

// Props
const props = defineProps({
  featuredProducts: {
    type: Array,
    default: () => []
  }
})

// Emits
defineEmits(['add-to-cart'])

// Estado reativo
const currentSlide = ref(0)
const showVideoModal = ref(false)
const videoLoading = ref(false)
const videoError = ref(false)
const videoPlayer = ref(null)
let autoSlideInterval = null

// Computed
const validFeaturedProducts = computed(() => {
  return props.featuredProducts?.length > 0 ? props.featuredProducts : []
})

// Métodos do carousel
const nextSlide = () => {
  if (validFeaturedProducts.value.length === 0) return
  currentSlide.value = (currentSlide.value + 1) % validFeaturedProducts.value.length
}

const prevSlide = () => {
  if (validFeaturedProducts.value.length === 0) return
  currentSlide.value = currentSlide.value === 0 
    ? validFeaturedProducts.value.length - 1 
    : currentSlide.value - 1
}

const goToSlide = (index) => {
  currentSlide.value = index
  resetAutoSlide()
}

const formatPrice = (price) => {
  return price.toFixed(2).replace('.', ',')
}

const handleImageError = (event) => {
  event.target.src = "data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 400 400'%3E%3Crect width='400' height='400' fill='%23f3f4f6'/%3E%3Ctext x='200' y='200' text-anchor='middle' font-family='Arial' font-size='24' fill='%236b7280'%3EProduto%3C/text%3E%3C/svg%3E"
}

const handleAvatarError = (event) => {
  // Fallback para gradiente colorido caso a imagem não carregue
  const colors = [
    'bg-gradient-to-br from-gold to-dark-gold',
    'bg-gradient-to-br from-blue-400 to-blue-600', 
    'bg-gradient-to-br from-green-400 to-green-600',
    'bg-gradient-to-br from-purple-400 to-purple-600'
  ]
  
  const randomColor = colors[Math.floor(Math.random() * colors.length)]
  event.target.style.display = 'none'
  
  // Criar div de fallback
  const fallbackDiv = document.createElement('div')
  fallbackDiv.className = `w-8 h-8 rounded-full border-2 border-white ${randomColor}`
  event.target.parentNode.replaceChild(fallbackDiv, event.target)
}

const scrollToProducts = () => {
  const element = document.querySelector('#products')
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' })
  }
}

// Métodos do vídeo
const openVideoModal = () => {
  showVideoModal.value = true
  videoError.value = false
  videoLoading.value = true
  
  // Resetar o vídeo para o início quando abrir o modal
  setTimeout(() => {
    if (videoPlayer.value) {
      videoPlayer.value.currentTime = 0
    }
  }, 100)
}

const closeVideoModal = () => {
  showVideoModal.value = false
  videoLoading.value = false
  videoError.value = false
  
  // Pausar o vídeo quando fechar o modal
  if (videoPlayer.value) {
    videoPlayer.value.pause()
  }
}

const handleVideoError = () => {
  videoLoading.value = false
  videoError.value = true
  console.error('Erro ao carregar vídeo: /videos/hellenic.mp4')
}

const retryVideo = () => {
  videoError.value = false
  videoLoading.value = true
  
  if (videoPlayer.value) {
    videoPlayer.value.load()
  }
}

const quickView = (product) => {
  // Implementar quick view
  console.log('Quick view:', product)
}

const getParticleStyle = (index) => {
  const positions = [
    { top: '10%', left: '10%' },
    { top: '20%', right: '15%' },
    { top: '60%', left: '5%' },
    { top: '80%', right: '20%' },
    { top: '30%', left: '70%' },
    { top: '50%', right: '10%' },
    { top: '15%', left: '50%' },
    { top: '70%', left: '60%' },
    { top: '40%', right: '5%' },
    { top: '85%', left: '30%' },
    { top: '25%', right: '40%' },
    { top: '55%', left: '20%' }
  ]
  
  return {
    ...positions[index - 1],
    animationDelay: `${index * 0.5}s`,
    animationDuration: '3s'
  }
}

// Auto-slide
const startAutoSlide = () => {
  if (validFeaturedProducts.value.length <= 1) return
  autoSlideInterval = setInterval(nextSlide, 5000)
}

const stopAutoSlide = () => {
  if (autoSlideInterval) {
    clearInterval(autoSlideInterval)
  }
}

const resetAutoSlide = () => {
  stopAutoSlide()
  startAutoSlide()
}

// Lifecycle
onMounted(() => {
  startAutoSlide()
  
  // Adicionar listener para tecla ESC
  const handleEsc = (e) => {
    if (e.key === 'Escape' && showVideoModal.value) {
      closeVideoModal()
    }
  }
  document.addEventListener('keydown', handleEsc)
  
  // Cleanup
  onUnmounted(() => {
    document.removeEventListener('keydown', handleEsc)
  })
})

onUnmounted(() => {
  stopAutoSlide()
})
</script>