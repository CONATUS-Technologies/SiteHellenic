<template>
  <footer class="relative bg-gradient-to-br from-dark-blue to-gray-900 text-white overflow-hidden">
    
    <!-- Padrão decorativo de fundo -->
    <div class="absolute inset-0 greek-pattern opacity-5"></div>
    
    <!-- Seção principal do footer -->
    <div class="container mx-auto px-4 py-20 relative z-10">
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-12">
        
        <!-- Informações da empresa -->
        <div class="lg:col-span-2">
          <div class="flex items-center space-x-4 mb-8 group">
            <div class="relative">
              <div class="text-5xl group-hover:scale-110 transition-transform duration-300">🏛️</div>
              <div class="absolute -inset-2 bg-gold/20 rounded-full opacity-0 group-hover:opacity-100 transition-opacity duration-300"></div>
            </div>
            <div>
              <h3 class="text-3xl font-bold text-gold-gradient font-cinzel">HELLENIC</h3>
              <p class="text-gray-300 font-cinzel text-lg">Experiência da Grécia Antiga</p>
            </div>
          </div>
          
          <p class="text-gray-300 mb-8 max-w-lg leading-relaxed text-lg">
            Descubra a majestade e sabedoria da Grécia Antiga através de uma jornada única 
            pela história, arquitetura e filosofia que moldaram a civilização ocidental.
          </p>
          
          <!-- Estatísticas -->
          <div class="grid grid-cols-3 gap-6 mb-8">
            <div class="text-center">
              <div class="text-2xl font-bold text-gold font-cinzel">500+</div>
              <div class="text-sm text-gray-400">Produtos</div>
            </div>
            <div class="text-center">
              <div class="text-2xl font-bold text-gold font-cinzel">10k+</div>
              <div class="text-sm text-gray-400">Clientes</div>
            </div>
            <div class="text-center">
              <div class="text-2xl font-bold text-gold font-cinzel">4.9</div>
              <div class="text-sm text-gray-400">Avaliação</div>
            </div>
          </div>
          
          <!-- Newsletter -->
          <div class="mb-8">
            <h4 class="text-xl font-semibold mb-4 font-cinzel text-gold flex items-center">
              <i class="fas fa-envelope mr-3"></i>
              Newsletter Exclusiva
            </h4>
            <p class="text-gray-400 mb-4 text-sm">
              Receba ofertas especiais, lançamentos e conteúdo sobre a Grécia Antiga
            </p>
            <div class="flex flex-col sm:flex-row gap-3">
              <input 
                type="email" 
                placeholder="Seu melhor e-mail"
                v-model="newsletterEmail"
                class="flex-1 px-4 py-3 bg-white/10 border border-white/20 rounded-lg focus:outline-none focus:border-gold focus:ring-2 focus:ring-gold/30 text-white placeholder-gray-400 backdrop-blur-sm transition-all duration-300"
              >
              <button 
                @click="subscribeNewsletter"
                class="btn-greek px-6 py-3 whitespace-nowrap"
              >
                <i class="fas fa-paper-plane mr-2"></i>
                Inscrever-se
              </button>
            </div>
            <p v-if="subscriptionMessage" class="text-sm text-green-400 mt-2 flex items-center">
              <i class="fas fa-check-circle mr-2"></i>
              {{ subscriptionMessage }}
            </p>
          </div>
        </div>
        
        <!-- Links rápidos -->
        <div>
          <h4 class="text-xl font-semibold mb-6 font-cinzel text-gold flex items-center">
            <i class="fas fa-sitemap mr-3"></i>
            Links Rápidos
          </h4>
          <ul class="space-y-4">
            <li v-for="link in quickLinks" :key="link.name">
              <a 
                :href="link.href" 
                class="text-gray-300 hover:text-gold transition-all duration-300 flex items-center group text-lg"
                @click="handleLinkClick($event, link.href)"
              >
                <i :class="link.icon" class="mr-4 w-5 text-gold/70 group-hover:text-gold transition-colors"></i>
                <span class="group-hover:translate-x-1 transition-transform">
                  {{ link.name }}
                </span>
              </a>
            </li>
          </ul>
        </div>
        
        <!-- Contato -->
        <div>
          <h4 class="text-xl font-semibold mb-6 font-cinzel text-gold flex items-center">
            <i class="fas fa-address-card mr-3"></i>
            Contato
          </h4>
          <ul class="space-y-4">
            <li class="flex items-start group">
              <i class="fas fa-map-marker-alt text-gold mr-4 mt-1 text-lg group-hover:animate-pulse"></i>
              <div>
                <p class="text-gray-300 font-semibold">Rua Atenas, 123</p>
                <p class="text-gray-400">Jardim Europa - São Paulo - SP</p>
                <p class="text-gray-400 text-sm">CEP: 01234-567</p>
              </div>
            </li>
            
            <li class="flex items-center group">
              <i class="fas fa-phone text-gold mr-4 text-lg group-hover:animate-pulse"></i>
              <div>
                <a href="tel:+5511999999999" class="text-gray-300 hover:text-gold transition-colors font-semibold">
                  (11) 99999-9999
                </a>
                <p class="text-gray-400 text-sm">Segunda à Sexta, 9h às 18h</p>
              </div>
            </li>
            
            <li class="flex items-center group">
              <i class="fas fa-envelope text-gold mr-4 text-lg group-hover:animate-pulse"></i>
              <div>
                <a href="mailto:contato@hellenic.com" class="text-gray-300 hover:text-gold transition-colors font-semibold">
                  contato@hellenic.com
                </a>
                <p class="text-gray-400 text-sm">Resposta em até 24h</p>
              </div>
            </li>
            
            <li class="flex items-center group">
              <i class="fab fa-whatsapp text-gold mr-4 text-lg group-hover:animate-pulse"></i>
              <div>
                <a href="https://wa.me/5511999999999" class="text-gray-300 hover:text-gold transition-colors font-semibold">
                  WhatsApp
                </a>
                <p class="text-gray-400 text-sm">Atendimento rápido</p>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
    
    <!-- Seção de redes sociais e certificações -->
    <div class="border-t border-white/10 backdrop-blur-sm">
      <div class="container mx-auto px-4 py-8 relative z-10">
        <div class="flex flex-col lg:flex-row justify-between items-center gap-8">
          
          <!-- Redes sociais -->
          <div class="flex flex-col sm:flex-row items-center gap-6">
            <span class="text-gray-300 font-cinzel text-lg">Siga-nos:</span>
            <div class="flex gap-4">
              <a 
                v-for="social in socialLinks" 
                :key="social.name"
                :href="social.url"
                target="_blank"
                rel="noopener noreferrer"
                class="group relative w-12 h-12 bg-white/10 hover:bg-gold rounded-full flex items-center justify-center transition-all duration-300 hover:scale-110 hover:shadow-lg"
                :title="social.name"
              >
                <i :class="social.icon" class="text-white text-lg group-hover:scale-110 transition-transform"></i>
                <div class="absolute -top-2 -right-2 w-4 h-4 bg-gold rounded-full opacity-0 group-hover:opacity-100 transition-opacity animate-pulse"></div>
              </a>
            </div>
          </div>
          
          <!-- Selos e certificações -->
          <div class="flex flex-wrap justify-center lg:justify-end gap-8">
            <div class="flex items-center gap-3 group cursor-pointer">
              <div class="w-10 h-10 bg-gold/20 rounded-full flex items-center justify-center group-hover:bg-gold/30 transition-colors">
                <i class="fas fa-shield-alt text-gold text-lg"></i>
              </div>
              <div>
                <span class="text-white font-semibold block">Compra Segura</span>
                <span class="text-gray-400 text-xs">SSL Certificado</span>
              </div>
            </div>
            
            <div class="flex items-center gap-3 group cursor-pointer">
              <div class="w-10 h-10 bg-gold/20 rounded-full flex items-center justify-center group-hover:bg-gold/30 transition-colors">
                <i class="fas fa-truck text-gold text-lg"></i>
              </div>
              <div>
                <span class="text-white font-semibold block">Entrega Garantida</span>
                <span class="text-gray-400 text-xs">Todo Brasil</span>
              </div>
            </div>
            
            <div class="flex items-center gap-3 group cursor-pointer">
              <div class="w-10 h-10 bg-gold/20 rounded-full flex items-center justify-center group-hover:bg-gold/30 transition-colors">
                <i class="fas fa-medal text-gold text-lg"></i>
              </div>
              <div>
                <span class="text-white font-semibold block">Qualidade Premium</span>
                <span class="text-gray-400 text-xs">Certificado</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Seção de pagamentos -->
    <div class="border-t border-white/10">
      <div class="container mx-auto px-4 py-6 relative z-10">
        <div class="flex flex-col md:flex-row justify-between items-center gap-6">
          <div class="text-center md:text-left">
            <h5 class="text-gold font-semibold mb-2 font-cinzel">Formas de Pagamento</h5>
            <div class="flex justify-center md:justify-start items-center gap-4">
              <i class="fab fa-cc-visa text-3xl text-blue-600 hover:scale-110 transition-transform cursor-pointer"></i>
              <i class="fab fa-cc-mastercard text-3xl text-red-500 hover:scale-110 transition-transform cursor-pointer"></i>
              <i class="fab fa-cc-amex text-3xl text-blue-400 hover:scale-110 transition-transform cursor-pointer"></i>
              <i class="fab fa-pix text-3xl text-teal-500 hover:scale-110 transition-transform cursor-pointer"></i>
              <span class="text-sm text-gray-400 bg-gray-800 px-2 py-1 rounded">12x sem juros</span>
            </div>
          </div>
          
          <div class="text-center md:text-right">
            <h5 class="text-gold font-semibold mb-2 font-cinzel">Segurança & Certificações</h5>
            <div class="flex justify-center md:justify-end items-center gap-3">
              <div class="bg-green-600 text-white px-2 py-1 rounded text-xs font-bold">SSL</div>
              <div class="bg-blue-600 text-white px-2 py-1 rounded text-xs font-bold">PCI DSS</div>
              <div class="bg-purple-600 text-white px-2 py-1 rounded text-xs font-bold">ISO 27001</div>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Copyright -->
    <div class="bg-black/30 border-t border-white/10">
      <div class="container mx-auto px-4 py-8 relative z-10">
        <div class="flex flex-col lg:flex-row justify-between items-center gap-6 text-sm">
          
          <!-- Copyright info -->
          <div class="text-center lg:text-left">
            <p class="text-gray-400 mb-2">
              &copy; {{ currentYear }} HELLENIC. Todos os direitos reservados.
            </p>
            <p class="text-gray-500 text-xs">
              CNPJ: 12.345.678/0001-90 | Razão Social: Hellenic Artefatos Ltda.
            </p>
          </div>
          
          <!-- Links legais -->
          <div class="flex flex-wrap justify-center gap-6 text-gray-400">
            <a href="#" class="hover:text-gold transition-colors hover:underline">
              Política de Privacidade
            </a>
            <a href="#" class="hover:text-gold transition-colors hover:underline">
              Termos de Uso
            </a>
            <a href="#" class="hover:text-gold transition-colors hover:underline">
              Trocas e Devoluções
            </a>
            <a href="#" class="hover:text-gold transition-colors hover:underline">
              FAQ
            </a>
            <a href="#" class="hover:text-gold transition-colors hover:underline">
              Trabalhe Conosco
            </a>
          </div>
        </div>
        
        <!-- Desenvolvido por -->
        <div class="text-center mt-6 pt-6 border-t border-white/10">
          <p class="text-gray-500 text-xs">
            Desenvolvido com <i class="fas fa-heart text-red-500 mx-1"></i> pela equipe HELLENIC
          </p>
        </div>
      </div>
    </div>

    <!-- Botão "Voltar ao Topo" -->
    <button 
      @click="scrollToTop"
      :class="[
        'fixed bottom-8 right-8 w-14 h-14 bg-gold hover:bg-dark-gold text-white rounded-full shadow-xl hover:shadow-2xl transition-all duration-300 z-40 flex items-center justify-center group',
        showBackToTop ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-4 pointer-events-none'
      ]"
    >
      <i class="fas fa-arrow-up text-lg group-hover:scale-110 transition-transform"></i>
      <div class="absolute inset-0 bg-gradient-to-r from-gold to-dark-gold rounded-full opacity-0 group-hover:opacity-100 transition-opacity"></div>
    </button>

    <!-- Chat Widget -->
    <div 
      :class="[
        'fixed bottom-8 left-8 transition-all duration-300 z-40',
        showChatWidget ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-4'
      ]"
    >
      <button 
        @click="toggleChat"
        class="w-14 h-14 bg-green-500 hover:bg-green-600 text-white rounded-full shadow-xl hover:shadow-2xl transition-all duration-300 flex items-center justify-center group relative"
      >
        <i :class="showChat ? 'fas fa-times' : 'fab fa-whatsapp'" class="text-xl group-hover:scale-110 transition-transform"></i>
        <div v-if="!showChat" class="absolute -top-1 -right-1 w-4 h-4 bg-red-500 rounded-full animate-pulse"></div>
      </button>
      
      <!-- Chat popup -->
      <div 
        v-if="showChat"
        class="absolute bottom-16 left-0 w-80 bg-white rounded-2xl shadow-2xl border border-gray-200 overflow-hidden"
      >
        <div class="bg-green-500 text-white p-4 flex items-center gap-3">
          <div class="w-10 h-10 bg-white/20 rounded-full flex items-center justify-center">
            <i class="fas fa-headset"></i>
          </div>
          <div>
            <h5 class="font-semibold">Atendimento Online</h5>
            <p class="text-xs opacity-90">Estamos aqui para ajudar!</p>
          </div>
        </div>
        
        <div class="p-4">
          <div class="mb-4">
            <div class="bg-gray-100 rounded-lg p-3 text-sm text-gray-700">
              Olá! Como podemos te ajudar hoje? 😊
            </div>
          </div>
          
          <div class="space-y-2">
            <button class="w-full text-left p-2 hover:bg-gray-50 rounded-lg text-sm transition-colors">
              📦 Rastrear meu pedido
            </button>
            <button class="w-full text-left p-2 hover:bg-gray-50 rounded-lg text-sm transition-colors">
              💳 Formas de pagamento
            </button>
            <button class="w-full text-left p-2 hover:bg-gray-50 rounded-lg text-sm transition-colors">
              🚚 Informações de entrega
            </button>
            <button class="w-full text-left p-2 hover:bg-gray-50 rounded-lg text-sm transition-colors">
              💬 Falar com atendente
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Elementos decorativos flutuantes -->
    <div class="absolute inset-0 overflow-hidden pointer-events-none">
      <div class="absolute top-20 left-10 w-2 h-2 bg-gold/30 rounded-full animate-pulse"></div>
      <div class="absolute top-40 right-20 w-1 h-1 bg-gold/40 rounded-full animate-pulse" style="animation-delay: 1s"></div>
      <div class="absolute bottom-32 left-1/4 w-3 h-3 bg-gold/20 rounded-full animate-pulse" style="animation-delay: 2s"></div>
      <div class="absolute top-1/3 right-1/3 w-1 h-1 bg-gold/50 rounded-full animate-pulse" style="animation-delay: 3s"></div>
    </div>
  </footer>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

// Estado reativo
const newsletterEmail = ref('')
const subscriptionMessage = ref('')
const showBackToTop = ref(false)
const showChatWidget = ref(true)
const showChat = ref(false)

// Computed
const currentYear = computed(() => new Date().getFullYear())

// Links e dados
const quickLinks = [
  { name: 'Início', href: '#home', icon: 'fas fa-home' },
  { name: 'Produtos', href: '#products', icon: 'fas fa-shopping-bag' },
  { name: 'Categorias', href: '#categories', icon: 'fas fa-th-large' },
  { name: 'Sobre Nós', href: '#about', icon: 'fas fa-info-circle' },
  { name: 'Blog', href: '#blog', icon: 'fas fa-blog' },
  { name: 'Contato', href: '#contact', icon: 'fas fa-envelope' },
  { name: 'FAQ', href: '#faq', icon: 'fas fa-question-circle' },
  { name: 'Carreiras', href: '#careers', icon: 'fas fa-briefcase' }
]

const socialLinks = [
  { 
    name: 'Facebook', 
    icon: 'fab fa-facebook-f', 
    url: 'https://facebook.com/hellenic',
    color: 'hover:bg-blue-600'
  },
  { 
    name: 'Instagram', 
    icon: 'fab fa-instagram', 
    url: 'https://instagram.com/hellenic',
    color: 'hover:bg-pink-600'
  },
  { 
    name: 'Twitter', 
    icon: 'fab fa-twitter', 
    url: 'https://twitter.com/hellenic',
    color: 'hover:bg-blue-400'
  },
  { 
    name: 'Pinterest', 
    icon: 'fab fa-pinterest', 
    url: 'https://pinterest.com/hellenic',
    color: 'hover:bg-red-600'
  },
  { 
    name: 'YouTube', 
    icon: 'fab fa-youtube', 
    url: 'https://youtube.com/hellenic',
    color: 'hover:bg-red-600'
  },
  { 
    name: 'TikTok', 
    icon: 'fab fa-tiktok', 
    url: 'https://tiktok.com/@hellenic',
    color: 'hover:bg-black'
  }
]

// Métodos
const subscribeNewsletter = () => {
  if (!newsletterEmail.value) {
    subscriptionMessage.value = 'Por favor, insira um e-mail válido'
    setTimeout(() => subscriptionMessage.value = '', 3000)
    return
  }
  
  // Validação básica de email
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  if (!emailRegex.test(newsletterEmail.value)) {
    subscriptionMessage.value = 'Por favor, insira um e-mail válido'
    setTimeout(() => subscriptionMessage.value = '', 3000)
    return
  }
  
  // Simular inscrição
  console.log('Newsletter subscription:', newsletterEmail.value)
  subscriptionMessage.value = 'Obrigado! Você foi inscrito com sucesso.'
  newsletterEmail.value = ''
  
  setTimeout(() => subscriptionMessage.value = '', 5000)
}

const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}

const toggleChat = () => {
  showChat.value = !showChat.value
}

const handleScroll = () => {
  showBackToTop.value = window.scrollY > 500
  
  // Mostrar chat widget após scroll
  if (window.scrollY > 200) {
    showChatWidget.value = true
  }
}

const handleLinkClick = (event, href) => {
  if (href.startsWith('#')) {
    event.preventDefault()
    const element = document.querySelector(href)
    if (element) {
      element.scrollIntoView({ behavior: 'smooth' })
    }
  }
}

// Lifecycle
onMounted(() => {
  window.addEventListener('scroll', handleScroll)
  
  // Mostrar chat widget após 3 segundos
  setTimeout(() => {
    showChatWidget.value = true
  }, 3000)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>