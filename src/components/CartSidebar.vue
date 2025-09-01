<template>
  <!-- Overlay -->
  <div 
    class="fixed inset-0 bg-black/60 z-50 transition-opacity duration-300 backdrop-blur-sm"
    @click="$emit('close')"
  ></div>

  <!-- Sidebar do Carrinho -->
  <div class="fixed right-0 top-0 h-full w-full max-w-lg bg-white shadow-2xl z-50 transform transition-transform duration-300 flex flex-col">
    
    <!-- Header do Carrinho -->
    <div class="flex items-center justify-between p-6 border-b border-gray-200 bg-gradient-to-r from-gold/5 to-dark-gold/5">
      <div>
        <h2 class="text-2xl font-bold text-dark-blue font-cinzel flex items-center">
          <i class="fas fa-shopping-cart mr-3 text-gold"></i>
          Seu Carrinho
        </h2>
        <p class="text-sm text-gray-600">{{ cartItems.length }} item{{ cartItems.length !== 1 ? 'ns' : '' }}</p>
      </div>
      <button 
        @click="$emit('close')"
        class="w-10 h-10 bg-gray-100 hover:bg-gray-200 text-gray-600 hover:text-gray-800 rounded-full flex items-center justify-center transition-all duration-300 hover:scale-110"
      >
        <i class="fas fa-times text-lg"></i>
      </button>
    </div>

    <!-- Progresso para frete grátis -->
    <div v-if="subtotal < freeShippingThreshold" class="p-4 bg-gradient-to-r from-blue-50 to-indigo-50 border-b border-gray-200">
      <div class="flex items-center justify-between mb-2">
        <span class="text-sm font-medium text-blue-800">
          <i class="fas fa-truck mr-2"></i>
          Frete grátis acima de R$ {{ formatPrice(freeShippingThreshold) }}
        </span>
        <span class="text-sm text-blue-600 font-bold">
          Faltam R$ {{ formatPrice(freeShippingThreshold - subtotal) }}
        </span>
      </div>
      <div class="w-full bg-blue-200 rounded-full h-2">
        <div 
          class="bg-gradient-to-r from-blue-500 to-indigo-600 h-2 rounded-full transition-all duration-500"
          :style="{ width: `${(subtotal / freeShippingThreshold) * 100}%` }"
        ></div>
      </div>
    </div>

    <!-- Conteúdo do Carrinho -->
    <div class="flex flex-col flex-1 overflow-hidden">
      
      <!-- Lista de itens -->
      <div class="flex-1 overflow-y-auto p-4">
        
        <!-- Carrinho Vazio -->
        <div v-if="cartItems.length === 0" class="text-center py-16">
          <div class="text-8xl text-gray-300 mb-6">🛒</div>
          <h3 class="text-2xl font-semibold text-gray-600 mb-3 font-cinzel">
            Seu carrinho está vazio
          </h3>
          <p class="text-gray-500 mb-8 text-lg">
            Que tal adicionar alguns produtos incríveis da nossa coleção?
          </p>
          <button 
            @click="$emit('close')"
            class="btn-greek px-8 py-3 text-lg"
          >
            <i class="fas fa-shopping-bag mr-2"></i>
            Explorar Produtos
          </button>
        </div>

        <!-- Itens do Carrinho -->
        <div v-else class="space-y-4">
          <div 
            v-for="(item, index) in cartItems" 
            :key="item.id"
            class="group bg-white border border-gray-200 rounded-xl p-4 hover:shadow-md transition-all duration-300 hover:border-gold/30"
            :style="{ animationDelay: `${index * 0.1}s` }"
            
          >
            <div class="flex items-start gap-4">
              <!-- Imagem do produto -->
              <div class="relative w-20 h-20 flex-shrink-0">
                <img 
                  :src="item.image" 
                  :alt="item.name"
                  class="w-full h-full object-cover rounded-lg"
                  @error="handleImageError"
                >
                <div class="absolute -top-2 -right-2 bg-gold text-white text-xs rounded-full w-6 h-6 flex items-center justify-center font-bold">
                  {{ item.quantity }}
                </div>
              </div>

              <!-- Informações do produto -->
              <div class="flex-1 min-w-0">
                <div class="flex items-start justify-between mb-2">
                  <div>
                    <h4 class="font-semibold text-gray-800 font-cinzel text-lg leading-tight">{{ item.name }}</h4>
                    <p class="text-sm text-gray-600">{{ item.category }}</p>
                  </div>
                  
                  <!-- Botão remover -->
                  <button 
                    @click="$emit('remove-item', item.id)"
                    class="opacity-0 group-hover:opacity-100 p-1 text-red-500 hover:text-red-700 hover:bg-red-50 rounded-full transition-all duration-300 ml-2"
                  >
                    <i class="fas fa-trash-alt text-sm"></i>
                  </button>
                </div>

                <div class="flex items-center justify-between">
                  <!-- Preço -->
                  <div>
                    <p class="text-xl font-bold text-gold">
                      R$ {{ formatPrice(item.price * item.quantity) }}
                    </p>
                    <p class="text-sm text-gray-500">
                      R$ {{ formatPrice(item.price) }} cada
                    </p>
                  </div>

                  <!-- Controles de quantidade -->
                  <div class="flex items-center bg-gray-100 rounded-lg p-1">
                    <button 
                      @click="updateQuantity(item.id, item.quantity - 1)"
                      class="w-8 h-8 flex items-center justify-center text-gray-600 hover:text-gold hover:bg-white rounded transition-all duration-300"
                      :disabled="item.quantity <= 1"
                    >
                      <i class="fas fa-minus text-xs"></i>
                    </button>
                    
                    <span class="w-12 text-center font-semibold text-gray-800">{{ item.quantity }}</span>
                    
                    <button 
                      @click="updateQuantity(item.id, item.quantity + 1)"
                      class="w-8 h-8 flex items-center justify-center text-gray-600 hover:text-gold hover:bg-white rounded transition-all duration-300"
                    >
                      <i class="fas fa-plus text-xs"></i>
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Resumo e Checkout -->
      <div v-if="cartItems.length > 0" class="border-t border-gray-200 bg-gray-50 p-6">
        
        <!-- Cupom de desconto -->
        <div class="mb-6">
          <div class="flex gap-2">
            <input 
              type="text" 
              placeholder="Código do cupom"
              v-model="couponCode"
              class="flex-1 px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:border-gold focus:ring-2 focus:ring-gold/20 transition-all duration-300"
            >
            <button 
              @click="applyCoupon"
              class="px-6 py-3 bg-gray-200 hover:bg-gray-300 rounded-lg transition-colors font-semibold text-gray-700 hover:text-gray-900"
            >
              Aplicar
            </button>
          </div>
          <p v-if="appliedCoupon" class="text-sm text-green-600 mt-2 flex items-center">
            <i class="fas fa-check-circle mr-2"></i>
            Cupom "{{ appliedCoupon }}" aplicado! Desconto de {{ discountPercent }}%
          </p>
        </div>

        <!-- Resumo de valores -->
        <div class="space-y-3 mb-6 bg-white rounded-lg p-4 border border-gray-200">
          <div class="flex justify-between text-gray-600">
            <span>Subtotal ({{ totalItems }} itens)</span>
            <span class="font-semibold">R$ {{ formatPrice(subtotal) }}</span>
          </div>
          
          <div v-if="discountAmount > 0" class="flex justify-between text-green-600">
            <span>Desconto ({{ discountPercent }}%)</span>
            <span class="font-semibold">-R$ {{ formatPrice(discountAmount) }}</span>
          </div>
          
          <div class="flex justify-between text-gray-600">
            <span class="flex items-center">
              Frete
              <i v-if="shipping === 0" class="fas fa-check-circle text-green-500 ml-2"></i>
            </span>
            <span class="font-semibold" :class="shipping === 0 ? 'text-green-600' : ''">
              {{ shipping === 0 ? 'Grátis!' : `R$ ${formatPrice(shipping)}` }}
            </span>
          </div>
          
          <hr class="border-gray-300">
          
          <div class="flex justify-between text-xl font-bold text-dark-blue">
            <span class="font-cinzel">Total</span>
            <span class="text-2xl">R$ {{ formatPrice(total) }}</span>
          </div>
          
          <div v-if="installmentPrice > 0" class="text-center text-sm text-gray-600">
            ou 12x de <span class="font-semibold text-gold">R$ {{ formatPrice(installmentPrice) }}</span> sem juros
          </div>
        </div>

        <!-- Botões de ação -->
        <div class="space-y-3">
          <button 
            class="w-full bg-gradient-to-r from-green-600 to-green-700 hover:from-green-700 hover:to-green-800 text-white py-4 rounded-xl font-semibold text-lg transition-all duration-300 hover:shadow-lg hover:scale-[1.02] flex items-center justify-center group"
            @click="finishOrder"
          >
            <i class="fas fa-credit-card mr-3 group-hover:animate-pulse"></i>
            Finalizar Compra
          </button>
          
          <button 
            @click="$emit('close')"
            class="w-full border-2 border-gold text-gold py-3 font-semibold hover:bg-gold hover:text-white transition-all duration-300 font-cinzel rounded-xl"
          >
            <i class="fas fa-shopping-bag mr-2"></i>
            Continuar Comprando
          </button>
        </div>

        <!-- Garantias e benefícios -->
        <div class="mt-6 pt-6 border-t border-gray-300">
          <h5 class="font-semibold text-gray-800 mb-3 font-cinzel text-center">
            <i class="fas fa-shield-alt text-gold mr-2"></i>
            Compre com Segurança
          </h5>
          <div class="grid grid-cols-2 gap-4 text-xs text-gray-600">
            <div class="flex items-center">
              <i class="fas fa-lock text-green-500 mr-2"></i>
              <span>Dados protegidos</span>
            </div>
            <div class="flex items-center">
              <i class="fas fa-undo text-blue-500 mr-2"></i>
              <span>Troca em 30 dias</span>
            </div>
            <div class="flex items-center">
              <i class="fas fa-shipping-fast text-purple-500 mr-2"></i>
              <span>Entrega rápida</span>
            </div>
            <div class="flex items-center">
              <i class="fas fa-headset text-orange-500 mr-2"></i>
              <span>Suporte 24/7</span>
            </div>
          </div>
        </div>

        <!-- Métodos de pagamento -->
        <div class="mt-4 text-center">
          <p class="text-xs text-gray-600 mb-2">Métodos de pagamento aceitos:</p>
          <div class="flex justify-center items-center space-x-3">
            <i class="fab fa-cc-visa text-2xl text-blue-600"></i>
            <i class="fab fa-cc-mastercard text-2xl text-red-600"></i>
            <i class="fab fa-cc-amex text-2xl text-blue-500"></i>
            <i class="fab fa-pix text-2xl text-teal-600"></i>
            <span class="text-xs bg-gray-200 px-2 py-1 rounded">Boleto</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// Props
const props = defineProps({
  cartItems: {
    type: Array,
    default: () => []
  }
})

// Estado reativo
const couponCode = ref('')
const appliedCoupon = ref('')
const discountPercent = ref(0)
const freeShippingThreshold = 200

// Emits
const emit = defineEmits(['close', 'update-quantity', 'remove-item'])

// Computed
const subtotal = computed(() => {
  return props.cartItems.reduce((total, item) => total + (item.price * item.quantity), 0)
})

const totalItems = computed(() => {
  return props.cartItems.reduce((total, item) => total + item.quantity, 0)
})

const discountAmount = computed(() => {
  return subtotal.value * (discountPercent.value / 100)
})

const shipping = computed(() => {
  return subtotal.value >= freeShippingThreshold ? 0 : 29.90
})

const total = computed(() => {
  return Math.max(0, subtotal.value - discountAmount.value + shipping.value)
})

const installmentPrice = computed(() => {
  return total.value / 12
})

// Métodos
const formatPrice = (price) => {
  return price.toFixed(2).replace('.', ',')
}

const handleImageError = (event) => {
  event.target.src = "data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 64 64'%3E%3Crect width='64' height='64' fill='%23f3f4f6'/%3E%3Ctext x='32' y='36' text-anchor='middle' font-family='Arial' font-size='10' fill='%236b7280'%3EImg%3C/text%3E%3C/svg%3E"
}

const updateQuantity = (productId, newQuantity) => {
  if (newQuantity <= 0) {
    emit('remove-item', productId)
    return
  }
  emit('update-quantity', productId, newQuantity)
}

const applyCoupon = () => {
  const validCoupons = {
    'WELCOME10': 10,
    'GREGA20': 20,
    'FRETEGRATIS': 5,
    'PRIMEIRA15': 15,
    'VIP25': 25
  }

  const code = couponCode.value.toUpperCase()
  
  if (validCoupons[code]) {
    appliedCoupon.value = code
    discountPercent.value = validCoupons[code]
    couponCode.value = ''
  } else if (couponCode.value) {
    alert('Cupom inválido ou expirado')
    couponCode.value = ''
  }
}

const finishOrder = () => {
  // Implementar lógica de finalização
  alert('Redirecionando para o checkout...')
  // Aqui você redirecionaria para a página de checkout
}
</script>