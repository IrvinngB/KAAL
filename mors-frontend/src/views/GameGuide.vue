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
    icon: 'summit',
    items: [
      'Llegá a la cima del K2 (8611m) vivo.',
      'Empezás a 5200m. Cada turno avanzás, descansás o te preparás.',
      'Si tu HP llega a 0, morís. No hay segundas oportunidades.',
      'La cima es opcional. Volver es obligatorio.',
    ],
  },
  {
    title: 'Acciones por turno',
    icon: 'bolt',
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
    icon: 'chart',
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
    icon: 'cloud',
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
    icon: 'skull',
    items: [
      '≥8000m: tu cuerpo se consume. O₂ drena 5% por turno.',
      'Willpower decae más rápido. Cada turno cuenta.',
      'Eventos críticos son más frecuentes.',
      'Primer entrada: +25 willpower (adrenalina de la cima).',
    ],
  },
  {
    title: 'Ciclo día/noche',
    icon: 'moon',
    items: [
      'Turnos 0–11: día. Condiciones normales.',
      'Turnos 12–23: noche. +30% costo de stamina, −10% confiabilidad del forecast.',
      'Acampar de noche recupera 15% menos stamina.',
      'Planificá tus movimientos según el ciclo.',
    ],
  },
  {
    title: 'Eventos',
    icon: 'warning',
    items: [
      'Máximo 1 evento por turno.',
      'Probabilidad aumenta con altitud y clima adverso.',
      'SECOND_WIND: recuperá stamina si pasaron 10+ turnos sin recuperar.',
      'Sherpa: −30% chance de eventos de caída o perderse.',
    ],
  },
  {
    title: 'Tips de supervivencia',
    icon: 'lightbulb',
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
          <div class="flex items-center gap-2.5 mb-3">
            <!-- Icon: Summit -->
            <svg v-if="section.icon === 'summit'" class="w-5 h-5 text-snow shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M8 3l4 8 5-5 7 14H0z"/>
            </svg>
            <!-- Icon: Bolt -->
            <svg v-else-if="section.icon === 'bolt'" class="w-5 h-5 text-warning shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M13 2L3 14h9l-1 8 10-12h-9l1-8z"/>
            </svg>
            <!-- Icon: Chart -->
            <svg v-else-if="section.icon === 'chart'" class="w-5 h-5 text-mors shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M3 3v18h18"/><path d="M18 17V9"/><path d="M13 17V5"/><path d="M8 17v-3"/>
            </svg>
            <!-- Icon: Cloud -->
            <svg v-else-if="section.icon === 'cloud'" class="w-5 h-5 text-ice shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M17.5 19H9a7 7 0 1 1 6.71-9h1.79a4.5 4.5 0 1 1 0 9Z"/>
            </svg>
            <!-- Icon: Skull -->
            <svg v-else-if="section.icon === 'skull'" class="w-5 h-5 text-danger shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <circle cx="12" cy="10" r="7"/><path d="M9 17v1a3 3 0 0 0 6 0v-1"/><circle cx="9" cy="10" r="1.5" fill="currentColor"/><circle cx="15" cy="10" r="1.5" fill="currentColor"/>
            </svg>
            <!-- Icon: Moon -->
            <svg v-else-if="section.icon === 'moon'" class="w-5 h-5 text-ice/60 shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"/>
            </svg>
            <!-- Icon: Warning -->
            <svg v-else-if="section.icon === 'warning'" class="w-5 h-5 text-danger shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M10.29 3.86L1.82 18a2 2 0 0 0 1.71 3h16.94a2 2 0 0 0 1.71-3L13.71 3.86a2 2 0 0 0-3.42 0z"/><line x1="12" y1="9" x2="12" y2="13"/><line x1="12" y1="17" x2="12.01" y2="17"/>
            </svg>
            <!-- Icon: Lightbulb -->
            <svg v-else-if="section.icon === 'lightbulb'" class="w-5 h-5 text-success shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M9 18h6"/><path d="M10 22h4"/><path d="M15.09 14c.18-.98.65-1.74 1.41-2.5A4.65 4.65 0 0 0 18 8 6 6 0 0 0 6 8c0 1 .23 2.23 1.5 3.5A4.61 4.61 0 0 1 8.91 14"/>
            </svg>
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
        <div class="flex items-center gap-2.5 mb-3">
          <svg class="w-5 h-5 text-ice shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/>
            <path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/>
            <line x1="8" y1="7" x2="16" y2="7"/>
            <line x1="8" y1="11" x2="14" y2="11"/>
          </svg>
          <h2 class="text-lg font-bold text-snow tracking-wide">Fórmula de Stamina</h2>
        </div>
        <div class="bg-white/5 rounded-lg p-4 font-mono text-xs text-ice/80 leading-relaxed overflow-x-auto">
          <p class="text-ice/80 mb-2 font-semibold">Costo por turno = BASE × alt_factor × weather × oxygen × willpower × role</p>
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-x-6 gap-y-1 text-ice/70">
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
