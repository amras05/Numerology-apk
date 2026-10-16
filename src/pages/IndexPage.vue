<template>
  <q-page class="q-pa-md flex flex-center" style="min-height: 100vh;">
    <div class="full-width app-container">
      
      <!-- Premium App Header -->
      <div class="text-center q-mb-lg header-spacing">
        <div class="row justify-center items-center q-gutter-sm">
          <q-icon name="auto_awesome" size="36px" color="primary" />
          <h1 class="text-h4 text-weight-bold q-my-none text-glow">Numerology Calculator</h1>
        </div>
        <p class="text-subtitle2 text-grey-5 q-mt-sm">
          Chaldean &amp; Pythagorean System
        </p>
      </div>

      <!-- Responsive Grid Container -->
      <div class="responsive-grid">
        
        <!-- Left Section (Inputs & Grid) -->
        <div class="left-section">
          <!-- Inputs Card (Name & DOB side-by-side) -->
          <div class="glass-card q-pa-lg q-mb-md">
            <div class="row q-col-gutter-md">
              <div class="col-12 col-sm-6">
                <q-input
                  v-model="nameInput"
                  outlined
                  dense
                  placeholder="ENTER NAME OR NUMBER..."
                  class="premium-input"
                  autofocus
                  clearable
                  autocomplete="off"
                  @clear="nameInput = ''"
                >
                  <template v-slot:prepend>
                    <q-icon name="person" color="primary" />
                  </template>
                </q-input>
              </div>
              
              <div class="col-12 col-sm-6">
                <q-input
                  v-model="dobInput"
                  outlined
                  dense
                  placeholder="DOB (DD-MM-YYYY)..."
                  class="premium-input"
                  mask="##-##-####"
                  clearable
                  autocomplete="off"
                  @clear="dobInput = ''"
                >
                  <template v-slot:prepend>
                    <q-icon name="cake" color="primary" />
                  </template>
                  <template v-slot:append>
                    <q-icon name="event" class="cursor-pointer" color="primary">
                      <q-popup-proxy cover transition-show="scale" transition-hide="scale">
                        <q-date v-model="dobInput" mask="DD-MM-YYYY">
                          <div class="row items-center justify-end">
                            <q-btn v-close-popup label="Close" color="primary" flat />
                          </div>
                        </q-date>
                      </q-popup-proxy>
                    </q-icon>
                  </template>
                </q-input>
              </div>
            </div>
            
            <div class="text-caption text-grey-5 q-mt-md text-center flex items-center justify-center q-gutter-md">
              <span class="flex items-center"><span class="vowel-dot q-mr-xs"></span> Vowel</span>
              <span class="flex items-center"><span class="consonant-dot q-mr-xs"></span> Consonant</span>
              <span class="flex items-center"><span class="number-dot q-mr-xs"></span> Number</span>
            </div>
          </div>

          <!-- Letters Grid Card -->
          <div class="glass-card q-pa-lg q-mb-md letters-grid-card" style="min-height: 200px; display: flex; flex-direction: column; justify-content: center;">
            <div v-if="nameInput.trim() === ''" class="text-center text-grey-6 py-lg">
              <q-icon name="fingerprint" size="64px" class="q-mb-md text-primary" />
              <div class="text-h6 text-weight-medium">Your Grid Awaits</div>
              <p class="text-body2 text-grey-6 max-width-sm q-mx-auto">
                Type a name or number in the input box above to reveal its numerological numbers and vibrational grid.
              </p>
            </div>

            <div v-else>
              <div v-for="(word, wordIndex) in words" :key="wordIndex" class="word-row">
                <div
                  v-for="(item, itemIndex) in word"
                  :key="itemIndex"
                  class="letter-card"
                  :class="item.isVowel ? 'state-vowel' : item.isDigit ? 'state-number' : 'state-consonant'"
                >
                  <!-- Letter Character -->
                  <div class="card-section letter-char">{{ item.char }}</div>
                  <!-- Chaldean Value -->
                  <div class="card-section method-val">
                    <span class="method-label">C:</span>
                    <span class="method-number">{{ item.chaldean }}</span>
                  </div>
                  <!-- Pythagorean Value -->
                  <div class="card-section method-val">
                    <span class="method-label">P:</span>
                    <span class="method-number">{{ item.pythagorean }}</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Right Section (Calculations Summary) -->
        <div class="right-section">
          
          <!-- DOB Results Card (Shown only if complete DOB is entered) -->
          <div v-if="isDobComplete" class="glass-card q-pa-lg q-mb-md dob-summary-card">
            <div class="text-h6 text-weight-bold text-primary q-mb-md flex items-center">
              <q-icon name="insights" size="24px" class="q-mr-xs" />
              Birth &amp; Destiny Numbers
            </div>
            
            <!-- Birth Number Row -->
            <div class="dob-stat-row q-mb-md">
              <div class="dob-stat-info">
                <div class="dob-stat-title text-glow-primary">Birth Number</div>
                <div class="dob-stat-formula text-grey-5">{{ birthCalc.formula }}</div>
              </div>
              <div class="num-circle dob-circle">{{ birthCalc.val }}</div>
            </div>
            
            <!-- Destiny Number Row -->
            <div class="dob-stat-row q-mb-md">
              <div class="dob-stat-info">
                <div class="dob-stat-title text-glow-primary">Destiny Number</div>
                <div class="dob-stat-formula text-grey-5">{{ destinyCalc.formula }}</div>
              </div>
              <div class="num-circle dob-circle">{{ destinyCalc.val }}</div>
            </div>

            <!-- Enemy Number Row -->
            <div class="dob-stat-row enemy-row q-pt-md">
              <div class="dob-stat-info">
                <div class="dob-stat-title text-rose-4">Enemy Number</div>
                <div class="dob-stat-formula text-grey-5">Based on Birth / Destiny</div>
              </div>
              <div class="num-circle dob-circle enemy-circle" :class="enemyCalc.val !== 'None' ? 'has-enemy' : ''">
                {{ enemyCalc.val }}
              </div>
            </div>
          </div>

          <!-- Name Calculation Cards -->
          <div class="glass-card overflow-hidden results-glass-card">
            <div class="results-container">
              
              <!-- Column 1: Total (Expression) -->
              <div class="result-column">
                <div class="col-title">Total</div>
                <div class="col-count text-primary">Full Vibration</div>
                
                <!-- Chaldean Row -->
                <div class="system-row">
                  <div class="system-label">Chaldean</div>
                  <div class="system-sum">{{ stats.total.chaldean }}</div>
                  <div class="num-circle">{{ stats.total.chaldeanReduced.display }}</div>
                </div>
                
                <!-- Pythagorean Row -->
                <div class="system-row">
                  <div class="system-label">Pythagorean</div>
                  <div class="system-sum">{{ stats.total.pythagorean }}</div>
                  <div class="num-circle">{{ stats.total.pythagoreanReduced.display }}</div>
                </div>
              </div>

              <!-- Column 2: Vowels (Soul Urge) -->
              <div class="result-column">
                <div class="col-title">Vowels</div>
                <div class="col-count text-emerald-5">{{ stats.vowels.count }} Vowel{{ stats.vowels.count === 1 ? '' : 's' }}</div>
                
                <!-- Chaldean Row -->
                <div class="system-row">
                  <div class="system-label">Chaldean</div>
                  <div class="system-sum">{{ stats.vowels.chaldean }}</div>
                  <div class="num-circle">{{ stats.vowels.chaldeanReduced.display }}</div>
                </div>
                
                <!-- Pythagorean Row -->
                <div class="system-row">
                  <div class="system-label">Pythagorean</div>
                  <div class="system-sum">{{ stats.vowels.pythagorean }}</div>
                  <div class="num-circle">{{ stats.vowels.pythagoreanReduced.display }}</div>
                </div>
              </div>

              <!-- Column 3: Consonants (Personality) -->
              <div class="result-column">
                <div class="col-title">Consonants</div>
                <div class="col-count text-grey-5">{{ stats.consonants.count }} Consonant{{ stats.consonants.count === 1 ? '' : 's' }}</div>
                
                <!-- Chaldean Row -->
                <div class="system-row">
                  <div class="system-label">Chaldean</div>
                  <div class="system-sum">{{ stats.consonants.chaldean }}</div>
                  <div class="num-circle">{{ stats.consonants.chaldeanReduced.display }}</div>
                </div>
                
                <!-- Pythagorean Row -->
                <div class="system-row">
                  <div class="system-label">Pythagorean</div>
                  <div class="system-sum">{{ stats.consonants.pythagorean }}</div>
                  <div class="num-circle">{{ stats.consonants.pythagoreanReduced.display }}</div>
                </div>
              </div>

            </div>
          </div>
        </div>

      </div>

      <!-- App Footer / Brand Info -->
      <div class="text-center text-grey-7 text-caption q-mt-xl q-mb-md">
        Numerology Calculator • Offline Local Execution
      </div>

    </div>
  </q-page>
</template>

<script setup>
import { ref, computed, watch } from 'vue'

const nameInput = ref('')
const dobInput = ref('')
const letters = ref([])

// Chaldean mapping table
const chaldeanTable = {
  A: 1, I: 1, J: 1, Q: 1, Y: 1,
  B: 2, K: 2, R: 2,
  C: 3, G: 3, L: 3, S: 3,
  D: 4, M: 4, T: 4,
  E: 5, H: 5, N: 5, X: 5,
  U: 6, V: 6, W: 6,
  O: 7, Z: 7,
  F: 8, P: 8
}

// Pythagorean mapping table
const pythagoreanTable = {
  A: 1, J: 1, S: 1,
  B: 2, K: 2, T: 2,
  C: 3, L: 3, U: 3,
  D: 4, M: 4, V: 4,
  E: 5, N: 5, W: 5,
  F: 6, O: 6, X: 6,
  G: 7, P: 7, Y: 7,
  H: 8, Q: 8, Z: 8,
  I: 9, R: 9
}

// Watch nameInput to sanitize and construct the letters grid (vowels auto-classified, numbers supported)
watch(nameInput, (newVal) => {
  if (!newVal) {
    letters.value = []
    return
  }
  
  // Allow letters, numbers, and spaces
  const sanitized = newVal.replace(/[^A-Za-z0-9\s]/g, '')
  if (sanitized !== newVal) {
    nameInput.value = sanitized
  }

  const chars = sanitized.split('')
  const newLetters = []
  let letterIndex = 0

  chars.forEach((char) => {
    if (char === ' ') {
      newLetters.push({ type: 'space' })
    } else if (/[0-9]/.test(char)) {
      const digitVal = parseInt(char, 10)
      newLetters.push({
        type: 'digit',
        char: char,
        chaldean: digitVal,
        pythagorean: digitVal,
        isVowel: false,
        isDigit: true,
        flatIndex: letterIndex++
      })
    } else {
      const upperChar = char.toUpperCase()
      // Auto classify: A, E, I, O, U are vowels, all others consonants. Clicking disabled.
      const isVowel = ['A', 'E', 'I', 'O', 'U'].includes(upperChar)

      newLetters.push({
        type: 'letter',
        char: upperChar,
        chaldean: chaldeanTable[upperChar] || 0,
        pythagorean: pythagoreanTable[upperChar] || 0,
        isVowel: isVowel,
        isDigit: false,
        flatIndex: letterIndex++
      })
    }
  })

  letters.value = newLetters
})

// Check if DOB input is fully entered (10 chars, e.g. DD-MM-YYYY)
const isDobComplete = computed(() => {
  return dobInput.value && dobInput.value.length === 10
})

// Single digit reducer helper for DOB calculations
function reduceToSingleDigit(num) {
  let val = num
  while (val > 9) {
    val = String(val).split('').reduce((acc, d) => acc + parseInt(d, 10), 0)
  }
  return val
}

// Birth Number Calculation: Day digits sum reduced to single digit
const birthCalc = computed(() => {
  if (!isDobComplete.value) return { val: 0, formula: '' }
  
  const dayStr = dobInput.value.substring(0, 2)
  const d1 = parseInt(dayStr[0], 10)
  const d2 = parseInt(dayStr[1], 10)
  const sum = d1 + d2
  
  if (sum > 9) {
    const finalVal = reduceToSingleDigit(sum)
    return {
      val: finalVal,
      formula: `${d1} + ${d2} = ${sum} => ${finalVal}`
    }
  } else {
    return {
      val: sum,
      formula: `${d1} + ${d2} = ${sum}`
    }
  }
})

// Destiny Number Calculation: All DOB digits sum reduced to single digit
const destinyCalc = computed(() => {
  if (!isDobComplete.value) return { val: 0, formula: '' }
  
  const digits = dobInput.value.replace(/-/g, '').split('').map(d => parseInt(d, 10))
  const sum = digits.reduce((acc, d) => acc + d, 0)
  
  const finalVal = reduceToSingleDigit(sum)
  const formulaLeft = digits.join('+')
  
  if (sum > 9) {
    return {
      val: finalVal,
      formula: `${formulaLeft} = ${sum} => ${finalVal}`
    }
  } else {
    return {
      val: sum,
      formula: `${formulaLeft} = ${sum}`
    }
  }
})

// Enemy Number Calculation logic:
// - If destiny or birth is 4: show 8
// - If destiny or birth is 8: show 9
// - If destiny or birth is 9: show 5
const enemyCalc = computed(() => {
  if (!isDobComplete.value) return { val: 'None' }
  
  const birth = birthCalc.value.val
  const destiny = destinyCalc.value.val
  const enemies = new Set()
  
  if (birth === 4 || destiny === 4) enemies.add(8)
  if (birth === 8 || destiny === 8) enemies.add(9)
  if (birth === 9 || destiny === 9) enemies.add(5)
  
  if (enemies.size === 0) {
    return { val: 'None' }
  }
  
  return {
    val: Array.from(enemies).join(', ')
  }
})

// Group flat letters array into words (separated by spaces) for rows
const words = computed(() => {
  const list = []
  let currentWord = []
  
  letters.value.forEach((item) => {
    if (item.type === 'space') {
      if (currentWord.length > 0) {
        list.push(currentWord)
        currentWord = []
      }
    } else {
      currentWord.push(item)
    }
  })
  
  if (currentWord.length > 0) {
    list.push(currentWord)
  }
  
  return list
})

// Single-digit reduction with Master Numbers (11, 22, 33) support
function reduceNumber(num) {
  if (num === 0) return { val: 0, display: '0', isMaster: false }
  
  let current = num
  while (current > 9) {
    if ([11, 22, 33].includes(current)) {
      const reducedDigit = Math.floor(current / 10) + (current % 10)
      return {
        val: current,
        display: `${current}/${reducedDigit}`,
        isMaster: true
      }
    }
    current = String(current).split('').reduce((acc, d) => acc + parseInt(d, 10), 0)
  }
  
  return {
    val: current,
    display: String(current),
    isMaster: false
  }
}

// Computes totals, vowel counts/sums, and consonant counts/sums
const stats = computed(() => {
  let chaldeanTotal = 0
  let pythagoreanTotal = 0
  
  let chaldeanVowels = 0
  let pythagoreanVowels = 0
  let vowelsCount = 0
  
  let chaldeanConsonants = 0
  let pythagoreanConsonants = 0
  let consonantsCount = 0

  letters.value.forEach((item) => {
    if (item.type === 'letter') {
      chaldeanTotal += item.chaldean
      pythagoreanTotal += item.pythagorean
      
      if (item.isVowel) {
        chaldeanVowels += item.chaldean
        pythagoreanVowels += item.pythagorean
        vowelsCount++
      } else {
        chaldeanConsonants += item.chaldean
        pythagoreanConsonants += item.pythagorean
        consonantsCount++
      }
    } else if (item.type === 'digit') {
      chaldeanTotal += item.chaldean
      pythagoreanTotal += item.pythagorean
    }
  })

  return {
    total: {
      chaldean: chaldeanTotal,
      chaldeanReduced: reduceNumber(chaldeanTotal),
      pythagorean: pythagoreanTotal,
      pythagoreanReduced: reduceNumber(pythagoreanTotal)
    },
    vowels: {
      count: vowelsCount,
      chaldean: chaldeanVowels,
      chaldeanReduced: reduceNumber(chaldeanVowels),
      pythagorean: pythagoreanVowels,
      pythagoreanReduced: reduceNumber(pythagoreanVowels)
    },
    consonants: {
      count: consonantsCount,
      chaldean: chaldeanConsonants,
      chaldeanReduced: reduceNumber(chaldeanConsonants),
      pythagorean: pythagoreanConsonants,
      pythagoreanReduced: reduceNumber(pythagoreanConsonants)
    }
  }
})
</script>

<style lang="scss" scoped>
// Legend dots
.vowel-dot, .consonant-dot, .number-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  display: inline-block;
}
.vowel-dot {
  background: #10b981;
}
.consonant-dot {
  background: #475569;
}
.number-dot {
  background: #f59e0b;
}
.text-glow {
  text-shadow: 0 0 10px rgba(99, 102, 241, 0.2);
}
</style>
