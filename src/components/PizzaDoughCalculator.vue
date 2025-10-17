<script setup>
import {computed, ref} from "vue"

// ====== 可調參數 ======
const doughWeightPerPiece = ref(210); // 每顆麵團重量（g）
const pieceCount = ref(24); // 麵團顆數
const flourFactor = ref(1.695); // 麵粉換算係數
const hydrationRate = ref(67) // 含水率 (67%)
const saltRate = ref(2.5) // 鹽比例 (2.5%)

// ====== 中間計算（盡量不四捨五入）======
const doughTotalWeight = computed(() =>
  doughWeightPerPiece.value * pieceCount.value
) // 麵團總重量（g）

const flourWeightRaw = computed(() =>
  doughTotalWeight.value / flourFactor.value
); // 麵粉 (g)

const purpleFlourWeightRaw = computed(() =>
  flourWeightRaw.value / 3
) // 紫粉（g）

const redFlourWeightRaw = computed(() =>
  flourWeightRaw.value * (2 / 3)
) // 紅粉（g）

const waterWeightRaw = computed(() =>
  flourWeightRaw.value * (hydrationRate.value / 100)
) // 水量 (g)

const saltWeightRaw = computed(() =>
  flourWeightRaw.value * saltRate.value / 100
) // 鹽量 (g)

// ====== 顯示層（這裡才四捨五入）======
const flourWeight = computed(() => Math.round(flourWeightRaw.value))
const purpleFlourWeight = computed(() => Math.round(purpleFlourWeightRaw.value))
const redFlourWeight = computed(() => Math.round(redFlourWeightRaw.value))
const waterWeight = computed(() => Math.round(waterWeightRaw.value))
const saltWeight = computed(() => Math.round(saltWeightRaw.value))


</script>

<template>
  <div class="min-h-screen bg-slate-950 text-slate-100">
    <!-- Header -->
    <header class="px-6 py-5 border-b border-slate-800 bg-slate-950/80 backdrop-blur sticky top-0 z-10">
      <h1 class="text-xl font-semibold tracking-wide">披薩麵團成分計算機</h1>
    </header>

    <main class="px-6 py-8 max-w-6xl mx-auto grid gap-6 lg:grid-cols-12">
      <!-- Controls -->
      <section class="lg:col-span-8 grid gap-6">
        <!-- 基本設定卡 -->
        <div class="rounded-2xl border border-slate-800 bg-slate-900/60 p-5 shadow-xl shadow-black/20">
          <h2 class="text-base font-medium mb-4">配方設定</h2>
          <div class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-3 gap-4">
            <!-- 每顆重量 -->
            <div>
              <label class="block text-sm text-slate-300 mb-1">每顆麵團重量</label>
              <div class="relative">
                <input v-model.number="doughWeightPerPiece" type="number" min="0" inputmode="decimal"
                       class="w-full rounded-xl bg-slate-950/60 border border-slate-800 px-3 py-2 pr-14 focus:outline-none focus:ring-2 focus:ring-indigo-500/60" />
                <span class="absolute right-5 top-1/2 -translate-y-1/2 text-xs text-slate-400">g</span>
              </div>
            </div>

            <!-- 顆數 -->
            <div>
              <label class="block text-sm text-slate-300 mb-1">顆數</label>
              <input v-model.number="pieceCount" type="number" min="1"
                     class="w-full rounded-xl bg-slate-950/60 border border-slate-800 px-3 py-2 focus:outline-none focus:ring-2 focus:ring-indigo-500/60" />
            </div>

            <!-- 麵粉換算係數 -->
            <div>
              <label class="block text-sm text-slate-300 mb-1">麵粉換算係數（flourFactor）</label>
              <input v-model.number="flourFactor" type="number" step="0.001" min="0.001"
                     class="w-full rounded-xl bg-slate-950/60 border border-slate-800 px-3 py-2 focus:outline-none focus:ring-2 focus:ring-indigo-500/60" />
            </div>

            <!-- 含水率 -->
            <div>
              <label class="block text-sm text-slate-300 mb-1">含水率</label>
<!--              <div class="flex items-center gap-3">-->
<!--                <input v-model.number="hydrationRatePct" type="number" min="0" max="100" step="0.5"-->
<!--                       class="w-28 rounded-xl bg-slate-950/60 border border-slate-800 px-3 py-2 focus:outline-none focus:ring-2 focus:ring-indigo-500/60" />-->
<!--                <input v-model.number="hydrationRatePct" type="range" min="50" max="85" step="0.5"-->
<!--                       class="flex-1" />-->
<!--                <span class="text-xs text-slate-400 w-10 text-right">%</span>-->
<!--              </div>-->
              <div class="relative">
                <input v-model.number="hydrationRate" type="number" min="0"
                       class="w-full rounded-xl bg-slate-950/60 border border-slate-800 px-3 py-2 pr-14 focus:outline-none focus:ring-2 focus:ring-indigo-500/60" />
                <span class="absolute right-5 top-1/2 -translate-y-1/2 text-xs text-slate-400">%</span>
              </div>
            </div>

            <!-- 鹽比例 -->
            <div>
              <label class="block text-sm text-slate-300 mb-1">鹽比例</label>
              <div class="relative">
                <input v-model.number="saltRate" type="number" min="0" step="0.1"
                       class="w-full rounded-xl bg-slate-950/60 border border-slate-800 px-3 py-2 pr-14 focus:outline-none focus:ring-2 focus:ring-indigo-500/60" />
                <span class="absolute right-5 top-1/2 -translate-y-1/2 text-xs text-slate-400">%
                </span>
              </div>
            </div>

            <!-- 粉類比例（1:2） -->
<!--            <div>-->
<!--              <label class="block text-sm text-slate-300 mb-1">粉類比例（紫:紅）</label>-->
<!--              <div class="flex items-center gap-3">-->
<!--                <input v-model.number="purpleRatio" type="number" min="0" step="1"-->
<!--                       class="w-20 rounded-xl bg-slate-950/60 border border-slate-800 px-3 py-2 focus:outline-none focus:ring-2 focus:ring-indigo-500/60" />-->
<!--                <span class="text-sm text-slate-400">:</span>-->
<!--                <input v-model.number="redRatio" type="number" min="0" step="1"-->
<!--                       class="w-20 rounded-xl bg-slate-950/60 border border-slate-800 px-3 py-2 focus:outline-none focus:ring-2 focus:ring-indigo-500/60" />-->
<!--                <span class="ml-auto text-xs text-slate-400">（會自動正規化為百分比）</span>-->
<!--              </div>-->
<!--            </div>-->
          </div>

        </div>

        <!-- 明細表卡 -->
        <div class="rounded-2xl border border-slate-800 bg-slate-900/60 p-5 shadow-xl shadow-black/20">
          <h2 class="text-base font-medium mb-4">配方明細</h2>
          <div class="overflow-hidden rounded-xl border border-slate-800">
            <table class="w-full text-sm">
              <thead class="bg-slate-900/80">
              <tr class="text-left text-slate-300">
                <th class="px-4 py-3">成分</th>
                <th class="px-4 py-3 w-32 text-right">重量</th>
              </tr>
              </thead>
              <tbody class="divide-y divide-slate-800">
              <tr class="font-medium">
                <td class="px-4 py-2">總粉量</td>
                <td class="px-4 py-2 text-right tabular-nums">{{ flourWeight }} g</td>
              </tr>
              <tr>
                <td class="px-4 py-2 pl-6">- 紫粉</td>
                <td class="px-4 py-2 text-right tabular-nums">{{ purpleFlourWeight }} g</td>
              </tr>
              <tr>
                <td class="px-4 py-2 pl-6">- 紅粉</td>
                <td class="px-4 py-2 text-right tabular-nums">{{ redFlourWeight }} g</td>
              </tr>

              <tr>
                <td class="px-4 py-2">水（常溫）</td>
                <td class="px-4 py-2 text-right tabular-nums">{{ waterWeight }} g</td>
              </tr>
              <tr>
                <td class="px-4 py-2">鹽</td>
                <td class="px-4 py-2 text-right tabular-nums">{{ saltWeight }} g</td>
              </tr>
              </tbody>
            </table>
          </div>
          <div class="flex text-xs text-slate-400 space-y-1 mt-3">
            <p>含水率：{{ hydrationRate }}%</p>
            <p class="px-2">|</p>
            <p>鹽：{{ saltRate }}%</p>
            <p class="px-2">|</p>
            <p>粉比：紫 3.33% / 紅 66.67%</p>
          </div>
        </div>
      </section>

    </main>
  </div>
</template>

<style scoped>

</style>
