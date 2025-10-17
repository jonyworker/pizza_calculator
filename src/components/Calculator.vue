<script setup>
import {computed, ref} from "vue"

// ====== 可調參數 ======
const doughWeightPerPiece = ref(210); // 每顆麵團重量（g）
const pieceCount = ref(24); // 麵團顆數
const flourFactor = ref(1.695); // 麵粉換算係數
const hydrationRate = ref(0.67) // 含水率 (67%)
const saltRate = ref(0.025) // 鹽比例 (2.5%)

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
  flourWeightRaw.value * hydrationRate.value
) // 水量 (g)

const saltWeightRaw = computed(() =>
  flourWeightRaw.value * saltRate.value
) // 鹽量 (g)

// ====== 顯示層（這裡才四捨五入）======
const flourWeight = computed(() => Math.round(flourWeightRaw.value))
const purpleFlourWeight = computed(() => Math.round(purpleFlourWeightRaw.value))
const redFlourWeight = computed(() => Math.round(redFlourWeightRaw.value))
const waterWeight = computed(() => Math.round(waterWeightRaw.value))
const saltWeight = computed(() => Math.round(saltWeightRaw.value))

</script>

<template>


  <div class="grid max-w-2xl grid-cols-1 gap-x-6 gap-y-8 sm:grid-cols-12">
    <div class="sm:col-span-6">
      <label for="first-name" class="block text-sm/6 font-medium text-gray-900 dark:text-white">麵團重量</label>
      <div class="mt-2">
        <input v-model.number="doughWeightPerPiece" type="number" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6 dark:bg-white/5 dark:text-white dark:outline-white/10 dark:placeholder:text-gray-500 dark:focus:outline-indigo-500" />
      </div>
    </div>

    <div class="sm:col-span-6">
      <label for="first-name" class="block text-sm/6 font-medium text-gray-900 dark:text-white">顆數</label>
      <div class="mt-2">
        <input v-model.number="pieceCount" type="number" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6 dark:bg-white/5 dark:text-white dark:outline-white/10 dark:placeholder:text-gray-500 dark:focus:outline-indigo-500" />
      </div>
    </div>
  </div>

  <p class="text-lg font-semibold text-gray-900 sm:text-xl dark:text-white">一球麵團重量:{{doughWeightPerPiece}}</p>
  <p class="text-lg font-semibold text-gray-900 sm:text-xl dark:text-white">顆數:{{pieceCount}}</p>
  <p class="text-lg font-semibold text-gray-900 sm:text-xl dark:text-white">doughTotalWeight:{{doughTotalWeight}}</p>
  <p class="text-lg font-semibold text-gray-900 sm:text-xl dark:text-white">flourWeight:{{flourWeight}}</p>
  <p class="text-lg font-semibold text-gray-900 sm:text-xl dark:text-white">紫粉:{{purpleFlourWeight}}g</p>
  <p class="text-lg font-semibold text-gray-900 sm:text-xl dark:text-white">紅粉:{{redFlourWeight}}g</p>


  <p class="text-lg font-semibold text-gray-900 sm:text-xl dark:text-white">水:{{waterWeight}}g</p>
  <p class="text-lg font-semibold text-gray-900 sm:text-xl dark:text-white">鹽:{{saltWeight}}g</p>





  <h2 class="text-4xl font-semibold tracking-tight text-gray-900 sm:text-5xl dark:text-white">Frequently asked questions</h2>


  <div class="grid grid-cols-1 gap-x-8 gap-y-10 border-b border-gray-900/10 pb-12 md:grid-cols-3 dark:border-white/10">

      <form class="bg-white shadow-sm outline outline-1 outline-gray-900/5 sm:rounded-xl md:col-span-3 dark:bg-gray-800/50 dark:shadow-none dark:-outline-offset-1 dark:outline-white/10">
        <div class="px-4 py-6 sm:p-8">
          <div class="grid max-w-2xl grid-cols-1 gap-x-6 gap-y-8 sm:grid-cols-6">
            <div class="sm:col-span-3">
              <label for="first-name" class="block text-sm/6 font-medium text-gray-900 dark:text-white">First name</label>
              <div class="mt-2">
                <input type="text" name="first-name" id="first-name" autocomplete="given-name" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6 dark:bg-white/5 dark:text-white dark:outline-white/10 dark:placeholder:text-gray-500 dark:focus:outline-indigo-500" />
              </div>
            </div>

            <div class="sm:col-span-3">
              <label for="last-name" class="block text-sm/6 font-medium text-gray-900 dark:text-white">Last name</label>
              <div class="mt-2">
                <input type="text" name="last-name" id="last-name" autocomplete="family-name" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6 dark:bg-white/5 dark:text-white dark:outline-white/10 dark:placeholder:text-gray-500 dark:focus:outline-indigo-500" />
              </div>
            </div>

            <div class="sm:col-span-4">
              <label for="email" class="block text-sm/6 font-medium text-gray-900 dark:text-white">Email address</label>
              <div class="mt-2">
                <input id="email" name="email" type="email" autocomplete="email" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6 dark:bg-white/5 dark:text-white dark:outline-white/10 dark:placeholder:text-gray-500 dark:focus:outline-indigo-500" />
              </div>
            </div>

            <div class="sm:col-span-3">
              <label for="country" class="block text-sm/6 font-medium text-gray-900 dark:text-white">Country</label>
              <div class="mt-2 grid grid-cols-1">
                <select id="country" name="country" autocomplete="country-name" class="col-start-1 row-start-1 w-full appearance-none rounded-md bg-white py-1.5 pl-3 pr-8 text-base text-gray-900 outline outline-1 -outline-offset-1 outline-gray-300 focus:outline focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6 dark:bg-white/5 dark:text-white dark:outline-white/10 dark:*:bg-gray-800 dark:focus:outline-indigo-500">
                  <option>United States</option>
                  <option>Canada</option>
                  <option>Mexico</option>
                </select>
                <ChevronDownIcon class="pointer-events-none col-start-1 row-start-1 mr-2 size-5 self-center justify-self-end text-gray-500 sm:size-4 dark:text-gray-400" aria-hidden="true" />
              </div>
            </div>

            <div class="col-span-full">
              <label for="street-address" class="block text-sm/6 font-medium text-gray-900 dark:text-white">Street address</label>
              <div class="mt-2">
                <input type="text" name="street-address" id="street-address" autocomplete="street-address" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6 dark:bg-white/5 dark:text-white dark:outline-white/10 dark:placeholder:text-gray-500 dark:focus:outline-indigo-500" />
              </div>
            </div>

            <div class="sm:col-span-2 sm:col-start-1">
              <label for="city" class="block text-sm/6 font-medium text-gray-900 dark:text-white">City</label>
              <div class="mt-2">
                <input type="text" name="city" id="city" autocomplete="address-level2" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6 dark:bg-white/5 dark:text-white dark:outline-white/10 dark:placeholder:text-gray-500 dark:focus:outline-indigo-500" />
              </div>
            </div>

            <div class="sm:col-span-2">
              <label for="region" class="block text-sm/6 font-medium text-gray-900 dark:text-white">State / Province</label>
              <div class="mt-2">
                <input type="text" name="region" id="region" autocomplete="address-level1" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6 dark:bg-white/5 dark:text-white dark:outline-white/10 dark:placeholder:text-gray-500 dark:focus:outline-indigo-500" />
              </div>
            </div>

            <div class="sm:col-span-2">
              <label for="postal-code" class="block text-sm/6 font-medium text-gray-900 dark:text-white">ZIP / Postal code</label>
              <div class="mt-2">
                <input type="text" name="postal-code" id="postal-code" autocomplete="postal-code" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6 dark:bg-white/5 dark:text-white dark:outline-white/10 dark:placeholder:text-gray-500 dark:focus:outline-indigo-500" />
              </div>
            </div>
          </div>
        </div>

        <div class="flex items-center justify-end gap-x-6 border-t border-gray-900/10 px-4 py-4 sm:px-8 dark:border-white/10">
          <button type="button" class="text-sm/6 font-semibold text-gray-900 dark:text-white">Cancel</button>
          <button type="submit" class="rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 dark:bg-indigo-500 dark:shadow-none dark:hover:bg-indigo-400 dark:focus-visible:outline-indigo-500">Save</button>
        </div>
      </form>

    </div>

</template>

<style scoped>

</style>
