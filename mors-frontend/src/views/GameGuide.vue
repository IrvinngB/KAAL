<script setup lang="ts">
import { useRouter } from 'vue-router'

const router = useRouter()

interface GuideSection {
  title: string
  icon: string
  items: string[]
}

const sections: GuideSection[] = [
  {
    title: 'Objetivo',
    icon: '🏔️',
    items: [
      'Llegá a la cima del K2 (8611m) vivo.',
      'Empezás a 5200m. Cada turno avanzás, descansás o te preparás.',
      'Si tu HP llega a 0, morís. No hay segundas oportunidades.',
      'La cima es opcional. Volver es obligatorio.',
    ],
  },
  {
    title: 'Acciones por turno',
    icon: '⚡',
    items: [
      'Avanzar normal — Progreso seguro, costo moderado.',
      'Avanzar agresivo — Más altitud, más riesgo (posible caída).',
      'Asegurar ruta — Gastá 1 cuerda para reducir riesgo de caídas futuras.',
      'Acampar — Recuperá stamina y temperatura. Gastá comida y gas.',
      'Usar oxígeno — +30% O₂, +10 willpower. Gastá 1 gas canister.',
      'Comer — Recuperá stamina según altitud. Gastá 1 ración.',
      'Descender — Bajás 200m. Recuperás un poco de temperatura.',
      'Descansar — +10 stamina, pero perdés tiempo.',
    ],
  },
  {
    title: 'Stats clave',
    icon: '📊',
    items: [
      'HP — Si llega a 0, morís. No se recupera normalmente.',
      'Stamina — Se gasta con cada acción. Si llega a 0, perdés HP cada turno.',
      'Willpower — Afecta costo de stamina (< 20 = +15% costo). Si llega a 0, perdés HP extra.',
      'Body temp — Si baja de 35°C, perdés HP por hipotermia.',
      'Altitude — Subir cuesta más stamina a mayor altura.',
    ],
  },
  {
    title: 'Clima',
    icon: '🌦️',
    items: [
      'CLEAR — Normal. Mejor recuperación al acampar.',
      'CLOUDY — +20% costo de stamina.',
      'WIND — +50% costo de stamina.',
      'STORM — +100% costo. Recuperación reducida al acampar.',
      'WHITEOUT — No podés avanzar sin cuerdas. Costo ×3.',
      'El forecast miente ~25%. Investigador tiene +25% confiabilidad.',
    ],
  },
  {
    title: 'Zona de la Muerte',
    icon: '💀',
    items: [
      '≥8000m: tu cuerpo se consume. O₂ drena 5% por turno.',
      'Willpower decae más rápido. Cada turno cuenta.',
      'Eventos críticos son más frecuentes.',
      'Primer entrada: +25 willpower (adrenalina de la cima).',
    ],
  },
  {
    title: 'Ciclo día/noche',
    icon: '🌙',
    items: [
      'Turnos 0–11: día. Condiciones normales.',
      'Turnos 12–23: noche. +30% costo de stamina, −10% confiabilidad del forecast.',
      'Acampar de noche recupera 15% menos stamina.',
      'Planificá tus movimientos según el ciclo.',
    ],
  },
  {
    title: 'Eventos',
    icon: '⚠️',
    items: [
      'Máximo 1 evento por turno.',
      'Probabilidad aumenta con altitud y clima adverso.',
      'SECOND_WIND: recuperá stamina si pasaron 10+ turnos sin recuperar.',
      'Sherpa: −30% chance de eventos de caída o perderse.',
    ],
  },
  {
    title: 'Tips de supervivencia',
    icon: '💡',
    items: [
      'No avances agresivo con stamina baja — el riesgo de caída es real.',
      'Asegurá ruta antes de whiteout — sin cuerdas, quedás trabado.',
      'Comé antes de que tu HP baje demasiado.',
      'El oxígeno es crucial en la zona de la muerte.',
      'Descender no es derrota — es estrategia.',
      'Cada rol tiene una estrategia distinta. Encontrá la tuya.',
    ],
  },
]

function onBack() {
  router.push('/')
}
</script>

<template>
  <div class="relative min-h-screen bg-[#03030a] flex flex-col items-center overflow-hidden px-4 py-8">
    <!-- Background gradient -->
    <div class="absolute inset-0 bg-gradient-to-b from-[#020205] via-[#08081a] to-peak/20" />
    <div class="absolute inset-0 bg-[radial-gradient(ellipse_at_center,transparent_30%,rgba(0,0,0,0.8)_100%)] pointer-events-none z-20" />

    <!-- Content -->
    <div class="relative z-30 w-full max-w-4xl animate-fade-in">
      <!-- Header -->
      <div class="text-center mb-8">
        <h1 class="text-4xl md:text-5xl font-black tracking-[0.15em] text-snow mb-2">
          Guía de Supervivencia
        </h1>
        <p class="text-ice/50 text-sm tracking-wider">
          Todo lo que necesitás saber para llegar a la cima del K2
        </p>
      </div>

      <!-- Sections Grid -->
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-8">
        <div
          v-for="section in sections"
          :key="section.title"
          class="glass-strong rounded-xl p-5 border border-white/10 backdrop-blur-xl"
        >
          <!-- Section Header -->
          <div class="flex items-center gap-2 mb-3">
            <span class="text-xl">{{ section.icon }}</span>
            <h2 class="text-lg font-bold text-snow tracking-wide">{{ section.title }}</h2>
          </div>

          <!-- Items -->
          <ul class="space-y-2">
            <li
              v-for="(item, index) in section.items"
              :key="index"
              class="text-ice/70 text-xs leading-relaxed flex items-start gap-2"
            >
              <span class="text-mors/60 mt-0.5 shrink-0">▸</span>
              <span>{{ item }}</span>
            </li>
          </ul>
        </div>
      </div>

      <!-- Formula reference -->
      <div class="glass-strong rounded-xl p-5 border border-white/10 backdrop-blur-xl mb-8">
        <div class="flex items-center gap-2 mb-3">
          <span class="text-xl">📐</span>
          <h2 class="text-lg font-bold text-snow tracking-wide">Fórmula de Stamina</h2>
        </div>
        <div class="bg-white/5 rounded-lg p-4 font-mono text-xs text-ice/80 leading-relaxed overflow-x-auto">
          <p class="text-mors/60 mb-2">Costo por turno = BASE × alt_factor × weather × oxygen × willpower × role</p>
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-x-6 gap-y-1 text-ice/50">
            <p>BASE = 12</p>
            <p>alt_factor = 1 + (alt / 8000)²</p>
            <p>CLEAR = 1.0</p>
            <p>CLOUDY = 1.2</p>
            <p>WIND = 1.5</p>
            <p>STORM = 2.0</p>
            <p>WHITEOUT = 3.0</p>
            <p>O₂ > 50% = 0.8 | O₂ < 30% = 1.4</p>
            <p>Willpower < 20 = 1.15</p>
            <p>Noche = 1.3×</p>
          </div>
        </div>
      </div>

      <!-- Back Button -->
      <div class="text-center">
        <button
          class="text-ice/50 hover:text-snow transition-colors duration-200 text-sm tracking-wider"
          @click="onBack"
        >
          ← Volver al Menú
        </button>
      </div>
    </div>
  </div>
</template>
