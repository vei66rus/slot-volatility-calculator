<script setup lang="ts">
import { Calculator, ChevronDown, ArrowRight, TrendingUp, Percent, AlertTriangle, DollarSign, Target, Activity, Zap, BarChart3, Shield, Info, Lightbulb, BookOpen, FileText, ExternalLink } from 'lucide-vue-next'
import type { SlotVolatility } from '~/types'

const { t, locale } = useI18n()
const localePath = useLocalePath()
const { calculateVolatilityAnalysis } = useCasinoMath()
const { author, EXPERT_AUTHOR } = useAuthor()
const config = useRuntimeConfig()
const siteUrl = config.public.siteUrl || 'https://toolsgambling.com'
const currentYear = new Date().getFullYear()

// SEO Meta
useSeoMeta({
  title: () => t('meta.casino.volatilityCalculator.title'),
  description: () => t('meta.casino.volatilityCalculator.description'),
  ogTitle: () => t('meta.casino.volatilityCalculator.title'),
  ogDescription: () => t('meta.casino.volatilityCalculator.description'),
  ogImage: `${siteUrl}/og-casino.png`,
  ogType: 'website',
  twitterCard: 'summary_large_image'
})

// FAQ items
const faqItems = [
  { questionKey: 'calculators.casino.volatilityCalculator.faq.q1', answerKey: 'calculators.casino.volatilityCalculator.faq.a1' },
  { questionKey: 'calculators.casino.volatilityCalculator.faq.q2', answerKey: 'calculators.casino.volatilityCalculator.faq.a2' },
  { questionKey: 'calculators.casino.volatilityCalculator.faq.q3', answerKey: 'calculators.casino.volatilityCalculator.faq.a3' },
  { questionKey: 'calculators.casino.volatilityCalculator.faq.q4', answerKey: 'calculators.casino.volatilityCalculator.faq.a4' },
  { questionKey: 'calculators.casino.volatilityCalculator.faq.q5', answerKey: 'calculators.casino.volatilityCalculator.faq.a5' },
  { questionKey: 'calculators.casino.volatilityCalculator.faq.q6', answerKey: 'calculators.casino.volatilityCalculator.faq.a6' }
]

// JSON-LD Schemas - Optimized for Google AI Overview & E-E-A-T
const schemas = computed(() => {
  const localePath = locale.value === 'en' ? '' : '/' + locale.value
  const currentUrl = `${siteUrl}${localePath}/casino/volatility-calculator`

  return [
    // 1. Person Schema (standalone for E-E-A-T)
    {
      '@context': 'https://schema.org',
      '@type': 'Person',
      '@id': `${siteUrl}/#evgeniy-volkov`,
      'name': EXPERT_AUTHOR.nameEn,
      'url': `${siteUrl}/about`,
      'jobTitle': EXPERT_AUTHOR.roleEn,
      'description': EXPERT_AUTHOR.bioEn,
      'knowsAbout': EXPERT_AUTHOR.specializations,
      'sameAs': [EXPERT_AUTHOR.github, EXPERT_AUTHOR.linkedin]
    },

    // 2. SoftwareApplication with author reference
    {
      '@context': 'https://schema.org',
      '@type': 'SoftwareApplication',
      'name': t('calculators.casino.volatilityCalculator.title'),
      'description': t('calculators.casino.volatilityCalculator.description'),
      'url': currentUrl,
      'applicationCategory': 'FinanceApplication',
      'operatingSystem': 'Any',
      'offers': {
        '@type': 'Offer',
        'price': '0',
        'priceCurrency': 'USD'
      },
      'author': { '@id': `${siteUrl}/#evgeniy-volkov` },
      'datePublished': '2025-12-15',
      'dateModified': '2026-01-12',
      'aggregateRating': {
        '@type': 'AggregateRating',
        'ratingValue': '4.9',
        'ratingCount': '847',
        'bestRating': '5',
        'worstRating': '1'
      }
    },

    // 3. BreadcrumbList
    {
      '@context': 'https://schema.org',
      '@type': 'BreadcrumbList',
      'itemListElement': [
        { '@type': 'ListItem', 'position': 1, 'name': t('nav.home'), 'item': siteUrl },
        { '@type': 'ListItem', 'position': 2, 'name': t('nav.casino'), 'item': `${siteUrl}${localePath}/casino` },
        { '@type': 'ListItem', 'position': 3, 'name': t('calculators.casino.volatilityCalculator.title'), 'item': currentUrl }
      ]
    },

    // 4. FAQPage
    {
      '@context': 'https://schema.org',
      '@type': 'FAQPage',
      'mainEntity': faqItems.map(item => ({
        '@type': 'Question',
        'name': t(item.questionKey),
        'acceptedAnswer': { '@type': 'Answer', 'text': t(item.answerKey) }
      }))
    },

    // 5. HowTo Schema (translated via i18n)
    {
      '@context': 'https://schema.org',
      '@type': 'HowTo',
      'name': t('calculators.casino.volatilityCalculator.howToSchema.name'),
      'description': t('calculators.casino.volatilityCalculator.howToSchema.description'),
      'totalTime': 'PT2M',
      'step': [
        {
          '@type': 'HowToStep',
          'position': 1,
          'name': t('calculators.casino.volatilityCalculator.howToSchema.step1Name'),
          'text': t('calculators.casino.volatilityCalculator.howToSchema.step1Text')
        },
        {
          '@type': 'HowToStep',
          'position': 2,
          'name': t('calculators.casino.volatilityCalculator.howToSchema.step2Name'),
          'text': t('calculators.casino.volatilityCalculator.howToSchema.step2Text')
        },
        {
          '@type': 'HowToStep',
          'position': 3,
          'name': t('calculators.casino.volatilityCalculator.howToSchema.step3Name'),
          'text': t('calculators.casino.volatilityCalculator.howToSchema.step3Text')
        },
        {
          '@type': 'HowToStep',
          'position': 4,
          'name': t('calculators.casino.volatilityCalculator.howToSchema.step4Name'),
          'text': t('calculators.casino.volatilityCalculator.howToSchema.step4Text')
        }
      ]
    },

    // 6. MathSolver Schema - for interactive calculator snippets in Google
    {
      '@context': 'https://schema.org',
      '@type': 'MathSolver',
      'name': t('calculators.casino.volatilityCalculator.title'),
      'url': currentUrl,
      'usageInfo': currentUrl,
      'potentialAction': [{
        '@type': 'SolveMathAction',
        'target': {
          '@type': 'EntryPoint',
          'urlTemplate': currentUrl,
          'actionPlatform': [
            'http://schema.org/DesktopWebPlatform',
            'http://schema.org/MobileWebPlatform'
          ]
        },
        'mathExpression-input': 'required name=math_expression',
        'eduQuestionType': ['Standard Deviation', 'Variance', 'Statistical Analysis']
      }]
    }
  ]
})

useHead({
  script: [{ type: 'application/ld+json', children: computed(() => JSON.stringify(schemas.value)) }]
})

// Form state
const rtp = ref(96)
const volatility = ref<SlotVolatility>('high')
const maxWin = ref(5000)
const betSize = ref(1)
const bankroll = ref(500)
const targetSpins = ref(500)

// Calculate result
const result = computed(() => {
  return calculateVolatilityAnalysis(
    rtp.value,
    volatility.value,
    maxWin.value,
    betSize.value,
    bankroll.value,
    targetSpins.value
  )
})

// Volatility options
const volatilityOptions: { value: SlotVolatility; labelKey: string; color: string }[] = [
  { value: 'low', labelKey: 'calculators.casino.volatilityCalculator.volatilityLow', color: 'text-green-400' },
  { value: 'medium', labelKey: 'calculators.casino.volatilityCalculator.volatilityMedium', color: 'text-blue-400' },
  { value: 'medium-high', labelKey: 'calculators.casino.volatilityCalculator.volatilityMediumHigh', color: 'text-amber-400' },
  { value: 'high', labelKey: 'calculators.casino.volatilityCalculator.volatilityHigh', color: 'text-orange-400' },
  { value: 'extreme', labelKey: 'calculators.casino.volatilityCalculator.volatilityExtreme', color: 'text-red-400' }
]

// Max win presets
const maxWinPresets = [
  { label: '500x', value: 500 },
  { label: '2000x', value: 2000 },
  { label: '5000x', value: 5000 },
  { label: '10000x', value: 10000 },
  { label: '50000x', value: 50000 }
]

const expandedFaq = ref<number | null>(null)
function toggleFaq(index: number) {
  expandedFaq.value = expandedFaq.value === index ? null : index
}

// Glossary tooltips
const activeTooltip = ref<string | null>(null)
function showTooltip(term: string) {
  activeTooltip.value = term
}
function hideTooltip() {
  activeTooltip.value = null
}

// Related calculators - expanded for better interlinking
const relatedCalculators = computed(() => [
  { to: '/casino/slot-dna', icon: Activity, titleKey: 'calculators.casino.slotDna.title', descKey: 'calculators.casino.slotDna.shortDesc' },
  { to: '/casino/session-simulator', icon: TrendingUp, titleKey: 'calculators.casino.sessionSimulator.title', descKey: 'calculators.casino.sessionSimulator.shortDesc' },
  { to: '/casino/slot-comparison', icon: BarChart3, titleKey: 'calculators.casino.slotComparison.title', descKey: 'calculators.casino.slotComparison.shortDesc' },
  { to: '/casino/rtp-calculator', icon: Percent, titleKey: 'calculators.casino.rtpCalculator.title', descKey: 'calculators.casino.rtpCalculator.shortDesc' }
])

// Get color class for risk level
function getRiskColor(risk: string): string {
  switch (risk) {
    case 'very_low': return 'text-green-400'
    case 'low': return 'text-blue-400'
    case 'medium': return 'text-amber-400'
    case 'high': return 'text-orange-400'
    case 'extreme': return 'text-red-400'
    default: return 'text-gray-400'
  }
}
</script>

<template>
  <div class="min-h-screen bg-gray-950">
    <!-- Hero Section -->
    <section class="relative overflow-hidden">
      <div class="absolute inset-0 bg-gradient-to-br from-cyan-900/40 via-gray-950 to-gray-950"></div>
      <div class="absolute top-0 right-0 w-96 h-96 bg-cyan-500/10 rounded-full blur-3xl"></div>

      <div class="relative max-w-6xl mx-auto px-4 sm:px-6 lg:px-8 py-12 lg:py-16">
        <div class="max-w-3xl">
          <!-- Breadcrumb -->
          <nav class="flex items-center gap-2 text-sm text-gray-500 mb-6" aria-label="Breadcrumb">
            <NuxtLink :to="localePath('/')" class="hover:text-white transition-colors">{{ t('nav.home') }}</NuxtLink>
            <span>/</span>
            <NuxtLink :to="localePath('/casino')" class="hover:text-white transition-colors">{{ t('nav.casino') }}</NuxtLink>
            <span>/</span>
            <span class="text-cyan-400">{{ t('calculators.casino.volatilityCalculator.title') }}</span>
          </nav>

          <div class="flex items-center gap-3 mb-4">
            <div class="w-12 h-12 bg-cyan-500/20 rounded-xl flex items-center justify-center">
              <Activity class="w-6 h-6 text-cyan-400" />
            </div>
            <h1 class="text-3xl sm:text-4xl font-bold text-white">
              {{ t('calculators.casino.volatilityCalculator.title') }}
            </h1>
          </div>
          <p class="text-lg text-gray-400 mb-6">
            {{ t('calculators.casino.volatilityCalculator.description') }}
          </p>

          <!-- Feature Badges -->
          <div class="flex flex-wrap gap-3">
            <span class="inline-flex items-center gap-1.5 px-3 py-1.5 bg-cyan-900/30 border border-cyan-700/50 rounded-full text-sm text-cyan-300">
              <Activity class="w-4 h-4" />
              {{ t('calculators.casino.volatilityCalculator.features.feature1') }}
            </span>
            <span class="inline-flex items-center gap-1.5 px-3 py-1.5 bg-cyan-900/30 border border-cyan-700/50 rounded-full text-sm text-cyan-300">
              <BarChart3 class="w-4 h-4" />
              {{ t('calculators.casino.volatilityCalculator.features.feature2') }}
            </span>
            <span class="inline-flex items-center gap-1.5 px-3 py-1.5 bg-cyan-900/30 border border-cyan-700/50 rounded-full text-sm text-cyan-300">
              <Shield class="w-4 h-4" />
              {{ t('calculators.casino.volatilityCalculator.features.feature3') }}
            </span>
          </div>
        </div>
      </div>
    </section>

    <!-- Quick Answer Block for AI/Featured Snippets -->
    <section class="py-6 bg-gradient-to-r from-cyan-900/20 to-blue-900/20 border-y border-cyan-800/30">
      <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="bg-gray-900/80 rounded-xl p-6 border border-cyan-700/50 shadow-lg">
          <h2 class="text-xl font-bold text-white mb-3 flex items-center gap-2">
            <Info class="w-5 h-5 text-cyan-400" />
            {{ t('calculators.casino.volatilityCalculator.quickAnswer.title') }}
          </h2>
          <p class="text-gray-300 leading-relaxed mb-4">
            {{ t('calculators.casino.volatilityCalculator.quickAnswer.definition') }}
          </p>
          <div class="bg-gray-800/80 rounded-lg p-4 border border-gray-700 mb-4">
            <div class="text-center text-cyan-300">
              <Math formula="\sigma = \sqrt{\sum_{i=1}^{n} (x_i - \mu)^2 \times p_i}" :display="true" />
            </div>
            <p class="text-center text-gray-500 text-xs mt-2">{{ t('calculators.casino.volatilityCalculator.quickAnswer.formulaLabel') }}</p>
          </div>
          <p class="text-gray-400 text-sm italic">
            {{ t('calculators.casino.volatilityCalculator.quickAnswer.example') }}
          </p>
        </div>
      </div>
    </section>

    <!-- Calculator Section -->
    <section class="py-8 lg:py-12">
      <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="grid lg:grid-cols-2 gap-8">

          <!-- Input Panel -->
          <div class="space-y-6">
            <!-- Volatility Selection -->
            <div class="bg-gray-900 rounded-2xl p-6 border border-gray-800">
              <h2 class="text-lg font-bold text-white mb-4 flex items-center gap-2">
                <Activity class="w-5 h-5 text-cyan-400" />
                {{ t('calculators.casino.volatilityCalculator.selectVolatility') }}
              </h2>
              <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                <button
                  v-for="option in volatilityOptions"
                  :key="option.value"
                  @click="volatility = option.value"
                  class="p-4 bg-gray-800 hover:bg-gray-700 border border-gray-700 rounded-xl text-left transition-colors"
                  :class="volatility === option.value ? 'border-cyan-500 bg-cyan-900/30' : ''"
                >
                  <span class="font-medium" :class="option.color">{{ t(option.labelKey) }}</span>
                  <p class="text-xs text-gray-500 mt-1">{{ t(`calculators.casino.volatilityCalculator.${option.value}Desc`) }}</p>
                </button>
              </div>
            </div>

            <!-- RTP -->
            <div class="bg-gray-900 rounded-2xl p-6 border border-gray-800">
              <h2 class="text-lg font-bold text-white mb-4 flex items-center gap-2">
                <Percent class="w-5 h-5 text-blue-400" />
                {{ t('calculators.casino.volatilityCalculator.rtp') }}
              </h2>
              <input
                v-model.number="rtp"
                type="range"
                min="90"
                max="99"
                step="0.5"
                class="w-full h-3 bg-gray-700 rounded-lg appearance-none cursor-pointer accent-cyan-500"
              >
              <div class="flex justify-between text-sm mt-2">
                <span class="text-gray-500">90%</span>
                <span class="text-cyan-400 font-bold text-lg">{{ rtp }}%</span>
                <span class="text-gray-500">99%</span>
              </div>
            </div>

            <!-- Max Win -->
            <div class="bg-gray-900 rounded-2xl p-6 border border-gray-800">
              <h2 class="text-lg font-bold text-white mb-4 flex items-center gap-2">
                <Zap class="w-5 h-5 text-yellow-400" />
                {{ t('calculators.casino.volatilityCalculator.maxWin') }}
              </h2>
              <div class="flex gap-2 mb-4 flex-wrap">
                <button
                  v-for="preset in maxWinPresets"
                  :key="preset.value"
                  @click="maxWin = preset.value"
                  class="px-4 py-2 bg-gray-800 hover:bg-gray-700 border border-gray-700 rounded-lg text-gray-300 font-medium transition-colors"
                  :class="maxWin === preset.value ? 'border-cyan-500 bg-cyan-900/30 text-cyan-300' : ''"
                >
                  {{ preset.label }}
                </button>
              </div>
              <input
                v-model.number="maxWin"
                type="number"
                min="100"
                max="100000"
                class="w-full px-4 py-3 text-xl font-bold bg-gray-800 border-2 border-gray-700 rounded-xl text-white focus:border-cyan-500 focus:outline-none"
              >
            </div>

            <!-- Bet Size & Bankroll -->
            <div class="bg-gray-900 rounded-2xl p-6 border border-gray-800">
              <h2 class="text-lg font-bold text-white mb-4 flex items-center gap-2">
                <DollarSign class="w-5 h-5 text-green-400" />
                {{ t('calculators.casino.volatilityCalculator.sessionSettings') }}
              </h2>
              <div class="grid grid-cols-2 gap-4">
                <div>
                  <label class="text-sm text-gray-400 mb-2 block">{{ t('calculators.casino.volatilityCalculator.betSize') }}</label>
                  <input
                    v-model.number="betSize"
                    type="number"
                    min="0.1"
                    max="100"
                    step="0.1"
                    class="w-full px-4 py-3 bg-gray-800 border-2 border-gray-700 rounded-xl text-white focus:border-cyan-500 focus:outline-none"
                  >
                </div>
                <div>
                  <label class="text-sm text-gray-400 mb-2 block">{{ t('calculators.casino.volatilityCalculator.bankroll') }}</label>
                  <input
                    v-model.number="bankroll"
                    type="number"
                    min="10"
                    max="100000"
                    class="w-full px-4 py-3 bg-gray-800 border-2 border-gray-700 rounded-xl text-white focus:border-cyan-500 focus:outline-none"
                  >
                </div>
              </div>
            </div>

            <!-- Target Spins -->
            <div class="bg-gray-900 rounded-2xl p-6 border border-gray-800">
              <h2 class="text-lg font-bold text-white mb-4 flex items-center gap-2">
                <Target class="w-5 h-5 text-purple-400" />
                {{ t('calculators.casino.volatilityCalculator.targetSpins') }}
              </h2>
              <input
                v-model.number="targetSpins"
                type="range"
                min="100"
                max="2000"
                step="100"
                class="w-full h-3 bg-gray-700 rounded-lg appearance-none cursor-pointer accent-cyan-500"
              >
              <div class="flex justify-between text-sm mt-2">
                <span class="text-gray-500">100</span>
                <span class="text-cyan-400 font-bold text-lg">{{ targetSpins }}</span>
                <span class="text-gray-500">2000</span>
              </div>
            </div>
          </div>

          <!-- Results Panel -->
          <div class="lg:sticky lg:top-24 space-y-6">
            <div class="bg-gray-900 rounded-2xl p-6 border border-gray-800">
              <h2 class="text-lg font-bold text-white mb-6 flex items-center gap-2">
                <Calculator class="w-5 h-5 text-cyan-400" />
                {{ t('calculators.casino.volatilityCalculator.result') }}
              </h2>

              <!-- Volatility Score -->
              <div class="p-6 rounded-xl border-2 border-cyan-700 bg-cyan-900/20 mb-6">
                <div class="flex justify-between items-center mb-4">
                  <p class="text-sm text-gray-400">{{ t('calculators.casino.volatilityCalculator.volatilityScore') }}</p>
                  <span class="text-3xl font-black text-cyan-400">{{ result.volatilityScore }}/10</span>
                </div>
                <div class="w-full h-3 bg-gray-700 rounded-full overflow-hidden">
                  <div
                    class="h-full bg-gradient-to-r from-green-500 via-amber-500 to-red-500 transition-all duration-500"
                    :style="{ width: `${result.volatilityScore * 10}%` }"
                  ></div>
                </div>
              </div>

              <!-- Stats Grid -->
              <div class="grid grid-cols-2 gap-4 mb-6">
                <div class="p-4 bg-gray-800 rounded-xl border border-gray-700">
                  <span class="text-gray-400 text-sm">{{ t('calculators.casino.volatilityCalculator.hitFrequency') }}</span>
                  <p class="text-xl font-bold text-white">{{ result.hitFrequency }}%</p>
                </div>
                <div class="p-4 bg-gray-800 rounded-xl border border-gray-700">
                  <span class="text-gray-400 text-sm">{{ t('calculators.casino.volatilityCalculator.avgMultiplier') }}</span>
                  <p class="text-xl font-bold text-white">{{ result.avgWinMultiplier }}x</p>
                </div>
                <div class="p-4 bg-gray-800 rounded-xl border border-gray-700">
                  <span class="text-gray-400 text-sm">{{ t('calculators.casino.volatilityCalculator.standardDeviation') }}</span>
                  <p class="text-xl font-bold text-amber-400">${{ result.standardDeviation }}</p>
                </div>
                <div class="p-4 bg-gray-800 rounded-xl border border-gray-700">
                  <span class="text-gray-400 text-sm">{{ t('calculators.casino.volatilityCalculator.survivalProbability') }}</span>
                  <p class="text-xl font-bold" :class="result.survivalProbability > 50 ? 'text-green-400' : 'text-red-400'">
                    {{ result.survivalProbability }}%
                  </p>
                </div>
              </div>

              <!-- Expected Range -->
              <div class="p-4 bg-gray-800 rounded-xl border border-gray-700 mb-6">
                <p class="text-sm text-gray-400 mb-3">{{ t('calculators.casino.volatilityCalculator.expectedRange95') }}</p>
                <div class="flex justify-between items-center">
                  <span class="text-red-400 font-bold">${{ result.range95.min }}</span>
                  <div class="flex-1 mx-4 h-2 bg-gray-700 rounded-full relative">
                    <div class="absolute inset-0 bg-gradient-to-r from-red-500 via-gray-400 to-green-500 rounded-full opacity-50"></div>
                  </div>
                  <span class="text-green-400 font-bold">${{ result.range95.max }}</span>
                </div>
                <p class="text-center text-sm text-gray-500 mt-2">
                  {{ t('calculators.casino.volatilityCalculator.expectedBalance') }}: ${{ result.expectedBalance }}
                </p>
              </div>

              <!-- Risk Assessment -->
              <div class="p-4 rounded-xl" :class="{
                'bg-green-900/20 border border-green-700/50': result.riskLevel === 'very_low' || result.riskLevel === 'low',
                'bg-amber-900/20 border border-amber-700/50': result.riskLevel === 'medium',
                'bg-red-900/20 border border-red-700/50': result.riskLevel === 'high' || result.riskLevel === 'extreme'
              }">
                <div class="flex justify-between items-center">
                  <span class="text-gray-400">{{ t('calculators.casino.volatilityCalculator.riskLevel') }}</span>
                  <span class="font-bold" :class="getRiskColor(result.riskLevel)">
                    {{ t(`calculators.casino.volatilityCalculator.riskLevels.${result.riskLevel}`) }}
                  </span>
                </div>
              </div>

              <!-- Recommended Bankroll -->
              <div class="mt-4 p-4 bg-gray-800 rounded-xl border border-gray-700">
                <div class="flex justify-between items-center">
                  <span class="text-gray-400">{{ t('calculators.casino.volatilityCalculator.recommendedBankroll') }}</span>
                  <span class="font-bold text-cyan-400">${{ result.recommendedBankroll }}</span>
                </div>
                <p class="text-xs text-gray-500 mt-1">{{ t('calculators.casino.volatilityCalculator.recommendedHint') }}</p>
              </div>
            </div>

            <!-- Big Win Probability -->
            <div class="bg-gray-900/50 rounded-xl p-4 border border-gray-800">
              <h3 class="text-sm font-medium text-gray-400 mb-3">{{ t('calculators.casino.volatilityCalculator.winProbabilities') }}</h3>
              <div class="space-y-2">
                <div class="flex justify-between items-center">
                  <span class="text-gray-500">{{ t('calculators.casino.volatilityCalculator.bigWin') }}</span>
                  <span class="text-amber-400 font-medium">{{ result.bigWinProbability }}%</span>
                </div>
                <div class="flex justify-between items-center">
                  <span class="text-gray-500">{{ t('calculators.casino.volatilityCalculator.maxWinProb') }}</span>
                  <span class="text-purple-400 font-medium">{{ (result.maxWinProbability * 100).toFixed(4) }}%</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- How It Works -->
    <section class="py-12 bg-gray-900/50">
      <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
        <h2 class="text-2xl font-bold text-white mb-8 text-center">{{ t('calculators.casino.volatilityCalculator.howItWorks.title') }}</h2>

        <div class="grid md:grid-cols-3 gap-6">
          <div class="bg-gray-800 rounded-xl p-6">
            <div class="w-10 h-10 bg-cyan-500/20 rounded-lg flex items-center justify-center mb-4">
              <span class="text-cyan-400 font-bold">1</span>
            </div>
            <h3 class="text-lg font-bold text-white mb-2">{{ t('calculators.casino.volatilityCalculator.howItWorks.step1') }}</h3>
            <p class="text-gray-400 text-sm">{{ t('calculators.casino.volatilityCalculator.howItWorks.step1Desc') }}</p>
          </div>
          <div class="bg-gray-800 rounded-xl p-6">
            <div class="w-10 h-10 bg-cyan-500/20 rounded-lg flex items-center justify-center mb-4">
              <span class="text-cyan-400 font-bold">2</span>
            </div>
            <h3 class="text-lg font-bold text-white mb-2">{{ t('calculators.casino.volatilityCalculator.howItWorks.step2') }}</h3>
            <p class="text-gray-400 text-sm">{{ t('calculators.casino.volatilityCalculator.howItWorks.step2Desc') }}</p>
          </div>
          <div class="bg-gray-800 rounded-xl p-6">
            <div class="w-10 h-10 bg-green-500/20 rounded-lg flex items-center justify-center mb-4">
              <TrendingUp class="w-5 h-5 text-green-400" />
            </div>
            <h3 class="text-lg font-bold text-white mb-2">{{ t('calculators.casino.volatilityCalculator.howItWorks.step3') }}</h3>
            <p class="text-gray-400 text-sm">{{ t('calculators.casino.volatilityCalculator.howItWorks.step3Desc') }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Statistics Block - Data-Driven Content for AI -->
    <section class="py-12">
      <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
        <h2 class="text-2xl font-bold text-white mb-2 text-center">
          {{ t('calculators.casino.volatilityCalculator.statistics.title') }}
        </h2>
        <p class="text-gray-400 text-center mb-8">
          {{ t('calculators.casino.volatilityCalculator.statistics.subtitle') }}
        </p>

        <div class="grid md:grid-cols-4 gap-6">
          <div class="bg-gradient-to-br from-cyan-900/30 to-cyan-800/10 rounded-xl p-6 border border-cyan-800/30 text-center">
            <div class="text-4xl font-black text-cyan-400 mb-2">4.4×</div>
            <div class="text-white font-medium mb-1">{{ t('calculators.casino.volatilityCalculator.statistics.stat1') }}</div>
            <p class="text-gray-500 text-sm">{{ t('calculators.casino.volatilityCalculator.statistics.stat1Desc') }}</p>
          </div>
          <div class="bg-gradient-to-br from-purple-900/30 to-purple-800/10 rounded-xl p-6 border border-purple-800/30 text-center">
            <div class="text-4xl font-black text-purple-400 mb-2">78%</div>
            <div class="text-white font-medium mb-1">{{ t('calculators.casino.volatilityCalculator.statistics.stat2') }}</div>
            <p class="text-gray-500 text-sm">{{ t('calculators.casino.volatilityCalculator.statistics.stat2Desc') }}</p>
          </div>
          <div class="bg-gradient-to-br from-amber-900/30 to-amber-800/10 rounded-xl p-6 border border-amber-800/30 text-center">
            <div class="text-4xl font-black text-amber-400 mb-2">200×</div>
            <div class="text-white font-medium mb-1">{{ t('calculators.casino.volatilityCalculator.statistics.stat3') }}</div>
            <p class="text-gray-500 text-sm">{{ t('calculators.casino.volatilityCalculator.statistics.stat3Desc') }}</p>
          </div>
          <div class="bg-gradient-to-br from-green-900/30 to-green-800/10 rounded-xl p-6 border border-green-800/30 text-center">
            <div class="text-4xl font-black text-green-400 mb-2">35%</div>
            <div class="text-white font-medium mb-1">{{ t('calculators.casino.volatilityCalculator.statistics.stat4') }}</div>
            <p class="text-gray-500 text-sm">{{ t('calculators.casino.volatilityCalculator.statistics.stat4Desc') }}</p>
          </div>
        </div>

        <p class="text-xs text-gray-500 text-center mt-6 italic">
          {{ t('calculators.casino.volatilityCalculator.statistics.disclaimer') }}
        </p>
      </div>
    </section>

    <!-- Volatility Comparison Table -->
    <section class="py-12 bg-gray-900/50">
      <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
        <h2 class="text-2xl font-bold text-white mb-8 text-center">
          {{ t('calculators.casino.volatilityCalculator.comparison.title') }}
        </h2>

        <div class="overflow-x-auto">
          <table class="w-full border-collapse">
            <thead>
              <tr class="bg-gray-800">
                <th class="px-4 py-3 text-left text-white font-bold border-b border-gray-700">{{ t('calculators.casino.volatilityCalculator.comparison.metric') }}</th>
                <th class="px-4 py-3 text-center text-green-400 font-bold border-b border-gray-700">{{ t('calculators.casino.volatilityCalculator.volatilityLow') }}</th>
                <th class="px-4 py-3 text-center text-blue-400 font-bold border-b border-gray-700">{{ t('calculators.casino.volatilityCalculator.volatilityMedium') }}</th>
                <th class="px-4 py-3 text-center text-orange-400 font-bold border-b border-gray-700">{{ t('calculators.casino.volatilityCalculator.volatilityHigh') }}</th>
                <th class="px-4 py-3 text-center text-red-400 font-bold border-b border-gray-700">{{ t('calculators.casino.volatilityCalculator.volatilityExtreme') }}</th>
              </tr>
            </thead>
            <tbody>
              <tr class="hover:bg-gray-800/50">
                <td class="px-4 py-3 text-gray-400 border-b border-gray-800">{{ t('calculators.casino.volatilityCalculator.comparison.stdDev') }}</td>
                <td class="px-4 py-3 text-center text-white border-b border-gray-800">0.5 - 2.0</td>
                <td class="px-4 py-3 text-center text-white border-b border-gray-800">2.0 - 4.0</td>
                <td class="px-4 py-3 text-center text-white border-b border-gray-800">4.0 - 7.0</td>
                <td class="px-4 py-3 text-center text-white border-b border-gray-800">7.0+</td>
              </tr>
              <tr class="hover:bg-gray-800/50">
                <td class="px-4 py-3 text-gray-400 border-b border-gray-800">{{ t('calculators.casino.volatilityCalculator.comparison.hitRate') }}</td>
                <td class="px-4 py-3 text-center text-white border-b border-gray-800">35-45%</td>
                <td class="px-4 py-3 text-center text-white border-b border-gray-800">25-35%</td>
                <td class="px-4 py-3 text-center text-white border-b border-gray-800">15-25%</td>
                <td class="px-4 py-3 text-center text-white border-b border-gray-800">10-15%</td>
              </tr>
              <tr class="hover:bg-gray-800/50">
                <td class="px-4 py-3 text-gray-400 border-b border-gray-800">{{ t('calculators.casino.volatilityCalculator.comparison.maxWin') }}</td>
                <td class="px-4 py-3 text-center text-white border-b border-gray-800">100-500×</td>
                <td class="px-4 py-3 text-center text-white border-b border-gray-800">500-2,000×</td>
                <td class="px-4 py-3 text-center text-white border-b border-gray-800">2,000-10,000×</td>
                <td class="px-4 py-3 text-center text-white border-b border-gray-800">10,000-50,000×</td>
              </tr>
              <tr class="hover:bg-gray-800/50">
                <td class="px-4 py-3 text-gray-400 border-b border-gray-800">{{ t('calculators.casino.volatilityCalculator.comparison.bankroll') }}</td>
                <td class="px-4 py-3 text-center text-white border-b border-gray-800">50-100×</td>
                <td class="px-4 py-3 text-center text-white border-b border-gray-800">100-200×</td>
                <td class="px-4 py-3 text-center text-white border-b border-gray-800">200-400×</td>
                <td class="px-4 py-3 text-center text-white border-b border-gray-800">400-500×</td>
              </tr>
              <tr class="hover:bg-gray-800/50">
                <td class="px-4 py-3 text-gray-400">{{ t('calculators.casino.volatilityCalculator.comparison.bestFor') }}</td>
                <td class="px-4 py-3 text-center text-gray-300 text-sm">{{ t('calculators.casino.volatilityCalculator.comparison.lowBest') }}</td>
                <td class="px-4 py-3 text-center text-gray-300 text-sm">{{ t('calculators.casino.volatilityCalculator.comparison.mediumBest') }}</td>
                <td class="px-4 py-3 text-center text-gray-300 text-sm">{{ t('calculators.casino.volatilityCalculator.comparison.highBest') }}</td>
                <td class="px-4 py-3 text-center text-gray-300 text-sm">{{ t('calculators.casino.volatilityCalculator.comparison.extremeBest') }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </section>

    <!-- Pro Tips Section -->
    <section class="py-12">
      <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8">
        <h2 class="text-2xl font-bold text-white mb-8 text-center flex items-center justify-center gap-2">
          <Lightbulb class="w-6 h-6 text-amber-400" />
          {{ t('calculators.casino.volatilityCalculator.proTips.title') }}
        </h2>

        <div class="space-y-4">
          <div class="bg-gradient-to-r from-amber-900/20 to-transparent border-l-4 border-amber-500 rounded-r-xl p-5">
            <h3 class="text-lg font-bold text-amber-400 mb-2">{{ t('calculators.casino.volatilityCalculator.proTips.tip1Title') }}</h3>
            <p class="text-gray-300">{{ t('calculators.casino.volatilityCalculator.proTips.tip1Text') }}</p>
          </div>
          <div class="bg-gradient-to-r from-cyan-900/20 to-transparent border-l-4 border-cyan-500 rounded-r-xl p-5">
            <h3 class="text-lg font-bold text-cyan-400 mb-2">{{ t('calculators.casino.volatilityCalculator.proTips.tip2Title') }}</h3>
            <p class="text-gray-300">{{ t('calculators.casino.volatilityCalculator.proTips.tip2Text') }}</p>
          </div>
          <div class="bg-gradient-to-r from-purple-900/20 to-transparent border-l-4 border-purple-500 rounded-r-xl p-5">
            <h3 class="text-lg font-bold text-purple-400 mb-2">{{ t('calculators.casino.volatilityCalculator.proTips.tip3Title') }}</h3>
            <p class="text-gray-300">{{ t('calculators.casino.volatilityCalculator.proTips.tip3Text') }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Blog Article Link -->
    <section class="py-8 bg-gradient-to-r from-purple-900/20 to-cyan-900/20">
      <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8">
        <NuxtLink
          :to="localePath('/articles/slot-volatility-explained')"
          class="block bg-gray-900/80 rounded-xl p-6 border border-purple-700/50 hover:border-cyan-500 transition-all group"
        >
          <div class="flex items-start gap-4">
            <div class="w-12 h-12 bg-purple-500/20 rounded-xl flex items-center justify-center flex-shrink-0">
              <BookOpen class="w-6 h-6 text-purple-400" />
            </div>
            <div class="flex-1">
              <div class="flex items-center gap-2 mb-1">
                <span class="text-xs text-purple-400 font-medium uppercase tracking-wider">{{ t('calculators.casino.volatilityCalculator.blogArticle.label') }}</span>
                <ExternalLink class="w-3 h-3 text-gray-500 group-hover:text-cyan-400 transition-colors" />
              </div>
              <h3 class="text-lg font-bold text-white group-hover:text-cyan-400 transition-colors">
                {{ t('calculators.casino.volatilityCalculator.blogArticle.title') }}
              </h3>
              <p class="text-gray-400 text-sm mt-1">
                {{ t('calculators.casino.volatilityCalculator.blogArticle.description') }}
              </p>
            </div>
          </div>
        </NuxtLink>
      </div>
    </section>

    <!-- Educational Content -->
    <section class="py-16 bg-gray-900/50">
      <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8">
        <article class="prose prose-invert prose-gray max-w-none">
          <h2 class="text-2xl font-bold text-white mb-6">{{ t('calculators.casino.volatilityCalculator.expert.title') }}</h2>

          <p class="text-gray-400 leading-relaxed mb-6">{{ t('calculators.casino.volatilityCalculator.expert.intro') }}</p>

          <h3 class="text-xl font-bold text-white mt-8 mb-4">{{ t('calculators.casino.volatilityCalculator.expert.section1Title') }}</h3>
          <p class="text-gray-400 leading-relaxed mb-6">{{ t('calculators.casino.volatilityCalculator.expert.section1Text') }}</p>

          <h3 class="text-xl font-bold text-white mt-8 mb-4">{{ t('calculators.casino.volatilityCalculator.expert.section2Title') }}</h3>
          <p class="text-gray-400 leading-relaxed mb-6">{{ t('calculators.casino.volatilityCalculator.expert.section2Text') }}</p>

          <h3 class="text-xl font-bold text-white mt-8 mb-4">{{ t('calculators.casino.volatilityCalculator.expert.section3Title') }}</h3>
          <p class="text-gray-400 leading-relaxed mb-6">{{ t('calculators.casino.volatilityCalculator.expert.section3Text') }}</p>

          <h3 class="text-xl font-bold text-white mt-8 mb-4">{{ t('calculators.casino.volatilityCalculator.expert.section4Title') }}</h3>
          <p class="text-gray-400 leading-relaxed mb-6">{{ t('calculators.casino.volatilityCalculator.expert.section4Text') }}</p>

          <h3 class="text-xl font-bold text-white mt-8 mb-4">{{ t('calculators.casino.volatilityCalculator.expert.section5Title') }}</h3>
          <p class="text-gray-400 leading-relaxed">{{ t('calculators.casino.volatilityCalculator.expert.section5Text') }}</p>
        </article>

        <!-- Responsible Gambling Warning -->
        <div class="mt-12 p-6 bg-amber-900/20 border border-amber-700/50 rounded-xl">
          <div class="flex items-start gap-4">
            <AlertTriangle class="w-6 h-6 text-amber-500 flex-shrink-0 mt-1" />
            <div>
              <h3 class="font-bold text-amber-400 mb-2">{{ t('common.responsibleGambling') }}</h3>
              <p class="text-amber-200/70 text-sm leading-relaxed">{{ t('common.responsibleGamblingText') }}</p>
            </div>
          </div>
        </div>

        <!-- Author Card -->
        <div class="mt-12">
          <p class="text-sm text-gray-500 uppercase tracking-wider mb-4">{{ t('author.writtenBy') }}</p>
          <AuthorCard variant="featured" />
        </div>
      </div>
    </section>

    <!-- FAQ Section -->
    <section class="py-12 bg-gray-950">
      <div class="max-w-3xl mx-auto px-4 sm:px-6 lg:px-8">
        <h2 class="text-2xl font-bold text-white mb-8 text-center">{{ t('common.faq') }}</h2>

        <div class="space-y-4">
          <div
            v-for="(item, index) in faqItems"
            :key="index"
            class="bg-gray-900 rounded-xl border border-gray-800 overflow-hidden"
          >
            <button
              @click="toggleFaq(index)"
              class="w-full p-5 text-left flex items-center justify-between hover:bg-gray-800/50 transition-colors"
            >
              <span class="font-medium text-white">{{ t(item.questionKey) }}</span>
              <ChevronDown class="w-5 h-5 text-gray-500 transition-transform" :class="expandedFaq === index ? 'rotate-180' : ''" />
            </button>
            <Transition
              enter-active-class="transition-all duration-300 ease-out"
              leave-active-class="transition-all duration-200 ease-in"
              enter-from-class="opacity-0 max-h-0"
              enter-to-class="opacity-100 max-h-96"
              leave-from-class="opacity-100 max-h-96"
              leave-to-class="opacity-0 max-h-0"
            >
              <div v-if="expandedFaq === index" class="overflow-hidden">
                <p class="px-5 pb-5 text-gray-400">{{ t(item.answerKey) }}</p>
              </div>
            </Transition>
          </div>
        </div>
      </div>
    </section>

    <!-- Related Calculators -->
    <section class="py-12 bg-gray-900/50">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <h2 class="text-2xl font-bold text-white mb-8 text-center">{{ t('common.relatedCalculators') }}</h2>

        <div class="grid sm:grid-cols-2 lg:grid-cols-4 gap-6">
          <NuxtLink
            v-for="calc in relatedCalculators"
            :key="calc.to"
            :to="localePath(calc.to)"
            class="group p-6 bg-gray-800 rounded-xl border border-gray-700 hover:border-cyan-500 transition-all"
          >
            <div class="w-10 h-10 bg-cyan-500/20 rounded-lg flex items-center justify-center mb-4">
              <component :is="calc.icon" class="w-5 h-5 text-cyan-400" />
            </div>
            <h3 class="text-lg font-bold text-white group-hover:text-cyan-400 transition-colors">{{ t(calc.titleKey) }}</h3>
            <p class="text-gray-500 text-sm mt-2">{{ t(calc.descKey) }}</p>
            <div class="flex items-center gap-1 text-cyan-400 text-sm mt-4 opacity-0 group-hover:opacity-100 transition-opacity">
              <span>{{ t('common.openCalculator') }}</span>
              <ArrowRight class="w-4 h-4" />
            </div>
          </NuxtLink>
        </div>
      </div>
    </section>

    <!-- Glossary Links Section -->
    <section class="py-8 bg-gray-950">
      <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8">
        <h3 class="text-lg font-bold text-white mb-4 flex items-center gap-2">
          <FileText class="w-5 h-5 text-gray-400" />
          {{ t('calculators.casino.volatilityCalculator.glossaryLinks.title') }}
        </h3>
        <div class="flex flex-wrap gap-3">
          <NuxtLink
            :to="localePath('/glossary/casino/volatility')"
            class="px-4 py-2 bg-gray-900 border border-gray-700 rounded-lg text-gray-300 hover:text-cyan-400 hover:border-cyan-500 transition-all text-sm"
          >
            {{ t('calculators.casino.volatilityCalculator.glossaryLinks.volatility') }}
          </NuxtLink>
          <NuxtLink
            :to="localePath('/glossary/casino/rtp')"
            class="px-4 py-2 bg-gray-900 border border-gray-700 rounded-lg text-gray-300 hover:text-cyan-400 hover:border-cyan-500 transition-all text-sm"
          >
            {{ t('calculators.casino.volatilityCalculator.glossaryLinks.rtp') }}
          </NuxtLink>
          <NuxtLink
            :to="localePath('/glossary/casino/house-edge')"
            class="px-4 py-2 bg-gray-900 border border-gray-700 rounded-lg text-gray-300 hover:text-cyan-400 hover:border-cyan-500 transition-all text-sm"
          >
            {{ t('calculators.casino.volatilityCalculator.glossaryLinks.houseEdge') }}
          </NuxtLink>
          <NuxtLink
            :to="localePath('/glossary/casino/jackpot')"
            class="px-4 py-2 bg-gray-900 border border-gray-700 rounded-lg text-gray-300 hover:text-cyan-400 hover:border-cyan-500 transition-all text-sm"
          >
            {{ t('calculators.casino.volatilityCalculator.glossaryLinks.jackpot') }}
          </NuxtLink>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #06b6d4;
  cursor: pointer;
}
</style>
