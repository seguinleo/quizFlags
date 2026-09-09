<script setup>
import { ref, computed, watch, onUnmounted } from 'vue'

const baseUrl = import.meta.env.BASE_URL

function loadStorage(key, fallback) {
  try {
    const value = localStorage.getItem(key)

    return value === null
      ? fallback
      : JSON.parse(value)
  } catch {
    return fallback
  }
}

function saveStorage(key, value) {
  localStorage.setItem(key, JSON.stringify(value))
}

function shuffle(array) {
  const result = [...array]

  for (let i = result.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1))
      ;[result[i], result[j]] = [result[j], result[i]]
  }

  return result
}

const darkMode = ref(loadStorage('darkMode', true))
const showCountryName = ref(false)
const showCountryCode = ref(false)
const sortAlphabetical = ref(false)
const showHelp = ref(false)
const hardcoreMode = ref(false)

const countries = {
  ad: "Andorre",
  ae: "Émirats arabes unis",
  af: "Afghanistan",
  ag: "Antigua-et-Barbuda",
  al: "Albanie",
  am: "Arménie",
  ao: "Angola",
  ar: "Argentine",
  at: "Autriche",
  au: "Australie",
  az: "Azerbaïdjan",
  ba: "Bosnie-Herzégovine",
  bb: "Barbade",
  bd: "Bangladesh",
  be: "Belgique",
  bf: "Burkina Faso",
  bg: "Bulgarie",
  bh: "Bahreïn",
  bi: "Burundi",
  bj: "Bénin",
  bn: "Brunei",
  bo: "Bolivie",
  br: "Brésil",
  bs: "Bahamas",
  bt: "Bhoutan",
  bw: "Botswana",
  by: "Biélorussie",
  bz: "Belize",
  ca: "Canada",
  cd: "République démocratique du Congo",
  cf: "République centrafricaine",
  cg: "République du Congo",
  ch: "Suisse",
  ci: "Côte d'Ivoire",
  cl: "Chili",
  cm: "Cameroun",
  cn: "Chine",
  co: "Colombie",
  cr: "Costa Rica",
  cu: "Cuba",
  cv: "Cap-Vert",
  cy: "Chypre",
  cz: "Tchéquie",
  de: "Allemagne",
  dj: "Djibouti",
  dk: "Danemark",
  dm: "Dominique",
  do: "République dominicaine",
  dz: "Algérie",
  ec: "Équateur",
  ee: "Estonie",
  eg: "Égypte",
  er: "Érythrée",
  es: "Espagne",
  et: "Éthiopie",
  fi: "Finlande",
  fj: "Fidji",
  fm: "Micronésie",
  fr: "France",
  ga: "Gabon",
  gb: "Royaume-Uni",
  gd: "Grenade",
  ge: "Géorgie",
  gh: "Ghana",
  gm: "Gambie",
  gn: "Guinée",
  gq: "Guinée équatoriale",
  gr: "Grèce",
  gt: "Guatemala",
  gw: "Guinée-Bissau",
  gy: "Guyana",
  hn: "Honduras",
  hr: "Croatie",
  ht: "Haïti",
  hu: "Hongrie",
  id: "Indonésie",
  ie: "Irlande",
  il: "Israël",
  in: "Inde",
  iq: "Irak",
  ir: "Iran",
  is: "Islande",
  it: "Italie",
  jm: "Jamaïque",
  jo: "Jordanie",
  jp: "Japon",
  ke: "Kenya",
  kg: "Kirghizistan",
  kh: "Cambodge",
  ki: "Kiribati",
  km: "Comores",
  kn: "Saint-Christophe-et-Niévès",
  kp: "Corée du Nord",
  kr: "Corée du Sud",
  kw: "Koweït",
  kz: "Kazakhstan",
  la: "Laos",
  lb: "Liban",
  lc: "Sainte-Lucie",
  li: "Liechtenstein",
  lk: "Sri Lanka",
  lr: "Liberia",
  ls: "Lesotho",
  lt: "Lituanie",
  lu: "Luxembourg",
  lv: "Lettonie",
  ly: "Libye",
  ma: "Maroc",
  mc: "Monaco",
  md: "Moldavie",
  me: "Monténégro",
  mg: "Madagascar",
  mh: "Îles Marshall",
  mk: "Macédoine du Nord",
  ml: "Mali",
  mm: "Myanmar (Birmanie)",
  mn: "Mongolie",
  mr: "Mauritanie",
  mt: "Malte",
  mu: "Maurice",
  mv: "Maldives",
  mw: "Malawi",
  mx: "Mexique",
  my: "Malaisie",
  mz: "Mozambique",
  na: "Namibie",
  ne: "Niger",
  ng: "Nigeria",
  ni: "Nicaragua",
  nl: "Pays-Bas",
  no: "Norvège",
  np: "Népal",
  nr: "Nauru",
  nz: "Nouvelle-Zélande",
  om: "Oman",
  pa: "Panama",
  pe: "Pérou",
  pg: "Papouasie-Nouvelle-Guinée",
  ph: "Philippines",
  pk: "Pakistan",
  pl: "Pologne",
  ps: "Palestine",
  pt: "Portugal",
  pw: "Palaos",
  py: "Paraguay",
  qa: "Qatar",
  ro: "Roumanie",
  rs: "Serbie",
  ru: "Russie",
  rw: "Rwanda",
  sa: "Arabie saoudite",
  sb: "Îles Salomon",
  sc: "Seychelles",
  sd: "Soudan",
  se: "Suède",
  sg: "Singapour",
  si: "Slovénie",
  sk: "Slovaquie",
  sl: "Sierra Leone",
  sm: "Saint-Marin",
  sn: "Sénégal",
  so: "Somalie",
  sr: "Suriname",
  ss: "Soudan du Sud",
  st: "Sao Tomé-et-Principe",
  sv: "Salvador",
  sy: "Syrie",
  sz: "Eswatini",
  td: "Tchad",
  tg: "Togo",
  th: "Thaïlande",
  tj: "Tadjikistan",
  tl: "Timor oriental",
  tm: "Turkménistan",
  tn: "Tunisie",
  to: "Tonga",
  tr: "Turquie",
  tt: "Trinité-et-Tobago",
  tv: "Tuvalu",
  tw: "Taïwan",
  tz: "Tanzanie",
  ua: "Ukraine",
  ug: "Ouganda",
  us: "États-Unis",
  uy: "Uruguay",
  uz: "Ouzbékistan",
  va: "Vatican",
  vc: "Saint-Vincent-et-les-Grenadines",
  ve: "Venezuela",
  vn: "Vietnam",
  vu: "Vanuatu",
  ws: "Samoa",
  ye: "Yémen",
  za: "Afrique du Sud",
  zm: "Zambie",
  zw: "Zimbabwe"
}

const countryCodes = Object.keys(countries)
const shuffledCountries = ref(shuffle(countryCodes))
const targetCountries = ref(shuffle(countryCodes))
const currentIndex = ref(0)

const score = ref(0)
const elapsedTime = ref(0)

const gameStarted = ref(false)
const gameOver = ref(false)

const foundCountries = ref(new Set())
const wrongCountry = ref(null)

const currentCountry = computed(
  () => targetCountries.value[currentIndex.value]
)

const currentCountryName = computed(
  () => countries[currentCountry.value]
)

const alphabeticalCountries = computed(() =>
  [...countryCodes].sort((a, b) =>
    countries[a].localeCompare(countries[b], 'fr')
  )
)

const displayedCountries = computed(() =>
  sortAlphabetical.value
    ? alphabeticalCountries.value
    : shuffledCountries.value
)

let timer = null

function startTimer() {
  stopTimer()

  timer = setInterval(() => {
    elapsedTime.value++
  }, 1000)
}

function stopTimer() {
  if (timer !== null) {
    clearInterval(timer)
    timer = null
  }
}

const bestScore = ref(
  loadStorage('bestScore', {
    score: 0,
    timer: 0
  })
)

function updateBestScore() {
  const currentScore = score.value
  const currentTime = elapsedTime.value

  const isBetter =
    currentScore > bestScore.value.score ||
    (
      currentScore === bestScore.value.score &&
      currentTime < bestScore.value.timer
    )

  if (!isBetter) return

  bestScore.value = {
    score: currentScore,
    timer: currentTime
  }

  saveStorage('bestScore', bestScore.value)
}

function selectCountry(code) {
  if (gameOver.value || foundCountries.value.has(code)) {
    return
  }

  if (!gameStarted.value) {
    gameStarted.value = true
    startTimer()
  }

  if (code !== currentCountry.value) {
    wrongCountry.value = code
    gameOver.value = true
    stopTimer()
    return
  }
  score.value++
  foundCountries.value.add(code)
  updateBestScore()

  if (score.value === targetCountries.value.length) {
    gameOver.value = true
    stopTimer()
    return
  }
  currentIndex.value++
}

function restartGame() {
  stopTimer()
  score.value = 0
  elapsedTime.value = 0
  gameStarted.value = false
  gameOver.value = false
  foundCountries.value = new Set()
  wrongCountry.value = null
  shuffledCountries.value = shuffle(countryCodes)
  targetCountries.value = shuffle(countryCodes)
  currentIndex.value = 0
}

function formatTime(seconds) {
  const minutes = Math.floor(seconds / 60)
  const remainingSeconds = seconds % 60

  return minutes
    ? `${minutes}min ${remainingSeconds}s`
    : `${remainingSeconds}s`
}

const formattedTime = computed(() =>
  formatTime(elapsedTime.value)
)

watch(
  darkMode,
  (value) => {
    saveStorage('darkMode', value)
    document.body.classList.toggle('light', !value)
  },
  { immediate: true }
)

onUnmounted(() => {
  stopTimer()
})
</script>

<template>
  <main>
    <header id="score">
      <span>
        <span v-if="gameOver && score === 196">GG ! (par contre ça sert à rien)</span>
        <span v-else-if="gameOver && score < 196">Partie terminée !</span>
        <span v-else-if="!gameOver">
          Trouvez le drapeau :
          <span class="primary">{{ currentCountryName }}</span>
        </span>
        <button id="restartBtn" @click="restartGame">
          🔄 Recommencer
        </button>
      </span>
      <span>
        Score : {{ score }} — {{ formattedTime }}, record :
        {{ bestScore.score }} points — {{ formatTime(bestScore.timer) }}
        <span v-if="bestScore.score === 196">👑</span>
        <span v-else-if="bestScore.score > 196">triiiicheuuuur !!!</span>
      </span>
    </header>
    <img v-if="showHelp" :src="`${baseUrl}map.png`" alt="Carte du monde" class="worldMap">
    <div id="flags">
      <button v-for="(code, i) in displayedCountries" :key="code" class="flag"
        :disabled="gameOver || foundCountries.has(code)" :class="{
          found: foundCountries.has(code),
          wrong: gameOver && code === wrongCountry,
          correct: gameOver && code === currentCountry,
          hardcore: hardcoreMode
        }" @click="selectCountry(code)" :aria-label="`flag-${i}`">
        <img :src="`${baseUrl}flags/${code}.svg`" width="50" alt="">
        <span v-if="showCountryName" class="flag-name">
          {{ countries[code] }}
        </span>
        <span v-if="showCountryCode" class="flag-code">
          {{ code.toUpperCase() }}
        </span>
      </button>
    </div>
    <div id="settings">
      <label class="row" for="darkMode">
        <span>dark mode</span>
        <span class="switch">
          <input type="checkbox" id="darkMode" v-model="darkMode" role="switch">
          <span class="toggle-thumb" aria-hidden="true">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" class="off">
              <rect x="12" y="6" width="1" height="12" />
            </svg>
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" class="on">
              <circle cx="12" cy="12" r="5" stroke-width="1" fill="none" />
            </svg>
          </span>
        </span>
      </label>
      <label class="row" for="showCountryName">
        <span>afficher nom pays</span>
        <span class="switch">
          <input type="checkbox" id="showCountryName" v-model="showCountryName" role="switch">
          <span class="toggle-thumb" aria-hidden="true">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" class="off">
              <rect x="12" y="6" width="1" height="12" />
            </svg>
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" class="on">
              <circle cx="12" cy="12" r="5" stroke-width="1" fill="none" />
            </svg>
          </span>
        </span>
      </label>
      <label class="row" for="showCountryCode">
        <span>afficher code pays</span>
        <span class="switch">
          <input type="checkbox" id="showCountryCode" v-model="showCountryCode" role="switch">
          <span class="toggle-thumb" aria-hidden="true">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" class="off">
              <rect x="12" y="6" width="1" height="12" />
            </svg>
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" class="on">
              <circle cx="12" cy="12" r="5" stroke-width="1" fill="none" />
            </svg>
          </span>
        </span>
      </label>
      <label class="row" for="sortAlphabetical">
        <span>tri alphabétique</span>
        <span class="switch">
          <input type="checkbox" id="sortAlphabetical" v-model="sortAlphabetical" role="switch">
          <span class="toggle-thumb" aria-hidden="true">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" class="off">
              <rect x="12" y="6" width="1" height="12" />
            </svg>
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" class="on">
              <circle cx="12" cy="12" r="5" stroke-width="1" fill="none" />
            </svg>
          </span>
        </span>
      </label>
      <label class="row" for="hardcoreMode">
        <span>mode extreme deluxe</span>
        <span class="switch">
          <input type="checkbox" id="hardcoreMode" v-model="hardcoreMode" role="switch">
          <span class="toggle-thumb" aria-hidden="true">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" class="off">
              <rect x="12" y="6" width="1" height="12" />
            </svg>
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" class="on">
              <circle cx="12" cy="12" r="5" stroke-width="1" fill="none" />
            </svg>
          </span>
        </span>
      </label>
      <label class="row" for="showHelp">
        <span>aide</span>
        <span class="switch">
          <input type="checkbox" id="showHelp" v-model="showHelp" role="switch">
          <span class="toggle-thumb" aria-hidden="true">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" class="off">
              <rect x="12" y="6" width="1" height="12" />
            </svg>
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" class="on">
              <circle cx="12" cy="12" r="5" stroke-width="1" fill="none" />
            </svg>
          </span>
        </span>
      </label>
    </div>
    <div id="footer">193 pays de l'ONU + Palestine + Taïwan + Vatican</div>
  </main>
</template>
