<script setup>
import { computed, onMounted, ref } from 'vue'
import heroOfficeCleaning from './assets/hero-office-cleaning.png'

const scopeAreas = [
  {
    label: 'Open space i stanowiska pracy',
    value: 'Biurka, blaty, przepierzenia, kosze, punkty dotykowe, widoczne zabrudzenia oraz ciągi komunikacyjne między stanowiskami.'
  },
  {
    label: 'Toalety i strefy sanitarne',
    value: 'Kabiny, umywalki, armatura, lustra, dozowniki, podłogi, klamki, włączniki oraz kontrola materiałów higienicznych.'
  },
  {
    label: 'Kuchnie i aneksy',
    value: 'Blaty, fronty, stoły, zlewy, zewnętrzne części sprzętów, podłogi oraz strefy socjalne po intensywnym dniu.'
  },
  {
    label: 'Sale konferencyjne i gabinety',
    value: 'Stoły, krzesła, ekrany, tablice, szkło, ślady po spotkaniach oraz szybkie przygotowanie kolejnej rezerwacji.'
  },
  {
    label: 'Recepcja i strefy gości',
    value: 'Lady, wejście, stoliki, meble, ekspozycje, podłogi i obszary, które budują pierwsze wrażenie po wejściu do firmy.'
  },
  {
    label: 'Podłogi, wykładziny i szkło',
    value: 'Odkurzanie, mopowanie, punktowe plamy, przeszklenia wewnętrzne, drzwi szklane i widoczne ślady użytkowania.'
  },
  {
    label: 'Prace po remoncie lub relokacji',
    value: 'Pył montażowy, ślady po ekipach technicznych, przygotowanie piętra do odbioru albo powrotu zespołu.'
  },
  {
    label: 'Wymagania specjalne',
    value: 'Nietypowe godziny, ograniczenia dostępu, strefy poufne, konkretne środki czystości albo praca przy obecnych pracownikach.'
  }
]

const operations = [
  { metric: '15 min', label: 'czas kwalifikacji zlecenia po danych z formularza' },
  { metric: '24 h', label: 'typowe okno startu dla pilnych realizacji w Warszawie' },
  { metric: '1 opiekun', label: 'jeden kontakt dla harmonogramu, uwag i rozliczeń' }
]

const currentPath = ref(window.location.hash.replace('#', '') || '/')
const selectedScope = ref('Pełny zakres biurowy')
const officeArea = ref('300-600 m2')
const visitMode = ref('Po godzinach')
const companyType = ref('Biuro')

const estimateSummary = computed(() => `${officeArea.value} / ${visitMode.value} / ${companyType.value}`)

const goTo = (path) => {
  window.location.hash = path
  currentPath.value = path
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

const scrollToSection = (sectionId) => {
  if (currentPath.value !== '/') {
    goTo('/')
    window.setTimeout(() => document.getElementById(sectionId)?.scrollIntoView({ behavior: 'smooth' }), 0)
    return
  }

  document.getElementById(sectionId)?.scrollIntoView({ behavior: 'smooth' })
}

onMounted(() => {
  window.addEventListener('hashchange', () => {
    currentPath.value = window.location.hash.replace('#', '') || '/'
  })
})
</script>

<template>
  <main class="workspace-shell">
    <aside class="side-rail">
      <button class="brand" type="button" @click="goTo('/')">
        <span class="brand-mark" aria-hidden="true">
          <svg viewBox="0 0 48 48">
            <path d="M10 34h28M14 28h22M18 22h16M25 9l11 11M31 9l5 5M12 39h24" />
          </svg>
        </span>
        <span>
          <strong>CzystePiętro</strong>
          <small>office cleaning ops</small>
        </span>
      </button>

      <nav aria-label="Główne">
        <button type="button" @click="scrollToSection('zakres')">Zakres</button>
        <button type="button" @click="scrollToSection('operacje')">Operacje</button>
        <button type="button" @click="goTo('/brief')">Brief</button>
      </nav>

      <div class="rail-contact">
        <span>kontakt@czystepietro.pl</span>
        <strong>+48 600 200 300</strong>
      </div>
    </aside>

    <section class="content-shell">
      <template v-if="currentPath === '/brief'">
        <section class="brief-view">
          <div class="brief-head">
            <button class="text-button" type="button" @click="goTo('/')">Powrót</button>
            <p class="eyebrow">Brief operacyjny</p>
            <h1>Wycena zaczyna się od zakresu, metrażu, rytmu pracy i ograniczeń dostępu.</h1>
          </div>

          <div class="brief-layout">
            <form class="brief-form">
              <div class="field-grid">
                <label>
                  Firma
                  <input type="text" placeholder="Nazwa firmy" />
                </label>
                <label>
                  Lokalizacja
                  <input type="text" placeholder="Warszawa, dzielnica, adres" />
                </label>
              </div>

              <div class="field-grid">
                <label>
                  Główny zakres
                  <select v-model="selectedScope">
                    <option>Pełny zakres biurowy</option>
                    <option>Toalety i strefy sanitarne</option>
                    <option>Kuchnie i aneksy</option>
                    <option>Sale konferencyjne</option>
                    <option>Sprzątanie po remoncie</option>
                    <option>Zakres niestandardowy</option>
                  </select>
                </label>
                <label>
                  Kontakt
                  <input type="tel" placeholder="+48 ..." />
                </label>
              </div>

              <fieldset class="segmented">
                <legend>Metraż</legend>
                <label v-for="area in ['do 180 m2', '180-300 m2', '300-600 m2', '600+ m2']" :key="area">
                  <input v-model="officeArea" type="radio" name="area" :value="area" />
                  <span>{{ area }}</span>
                </label>
              </fieldset>

              <fieldset class="segmented">
                <legend>Okno pracy</legend>
                <label v-for="mode in ['Rano', 'W godzinach pracy', 'Po godzinach', 'Weekend']" :key="mode">
                  <input v-model="visitMode" type="radio" name="mode" :value="mode" />
                  <span>{{ mode }}</span>
                </label>
              </fieldset>

              <fieldset class="segmented">
                <legend>Typ przestrzeni</legend>
                <label v-for="type in ['Biuro', 'Showroom', 'Cowork', 'Siedziba HQ']" :key="type">
                  <input v-model="companyType" type="radio" name="type" :value="type" />
                  <span>{{ type }}</span>
                </label>
              </fieldset>

              <label>
                Szczegółowe wymagania
                <textarea rows="6" placeholder="Toalety, kuchnie, sale, recepcja, wykładziny, szkło, ograniczenia dostępu, środki czystości, nietypowe zabrudzenia"></textarea>
              </label>

              <button type="button">Wyślij brief do wyceny</button>
            </form>

            <aside class="brief-summary">
              <span class="status-dot">Wstępny zakres</span>
              <h2>{{ selectedScope }}</h2>
              <strong>wycena po briefie</strong>
              <p>Zakres ustalamy indywidualnie, bo każde biuro ma inny ruch, inne strefy krytyczne i inne wymagania techniczne.</p>
              <dl>
                <div>
                  <dt>Tryb</dt>
                  <dd>{{ estimateSummary }}</dd>
                </div>
                <div>
                  <dt>Model</dt>
                  <dd>jednorazowo lub cyklicznie</dd>
                </div>
                <div>
                  <dt>Zakres</dt>
                  <dd>dobierany indywidualnie</dd>
                </div>
              </dl>
            </aside>
          </div>
        </section>
      </template>

      <template v-else>
        <section class="hero-grid">
          <div class="hero-panel">
            <p class="eyebrow">Serwis czystości dla biur</p>
            <h1>Czystość biura traktowana jak proces, nie jednorazowa obietnica.</h1>
            <p>
              CzystePiętro planuje i realizuje czyszczenie powierzchni firmowych według metrażu,
              godzin dostępu, stref krytycznych i raportów po pracy.
            </p>
            <div class="hero-actions">
              <button type="button" @click="goTo('/brief')">Utwórz brief</button>
              <button type="button" @click="scrollToSection('zakres')">Zobacz zakres</button>
            </div>
          </div>

          <div class="estimate-console">
            <div class="console-top">
              <span>Wycena startowa</span>
              <strong>brief → wycena</strong>
              <small>{{ selectedScope }} · {{ officeArea }}</small>
            </div>
            <label>
              Główny zakres
              <select v-model="selectedScope">
                <option>Pełny zakres biurowy</option>
                <option>Toalety i strefy sanitarne</option>
                <option>Kuchnie i aneksy</option>
                <option>Sale konferencyjne</option>
                <option>Sprzątanie po remoncie</option>
                <option>Zakres niestandardowy</option>
              </select>
            </label>
            <label>
              Metraż
              <select v-model="officeArea">
                <option>do 180 m2</option>
                <option>180-300 m2</option>
                <option>300-600 m2</option>
                <option>600+ m2</option>
              </select>
            </label>
            <label>
              Okno pracy
              <select v-model="visitMode">
                <option>Rano</option>
                <option>W godzinach pracy</option>
                <option>Po godzinach</option>
                <option>Weekend</option>
              </select>
            </label>
            <div class="console-checklist">
              <span>W briefie uwzględnimy</span>
              <ul>
                <li>metraż i typ podłóg</li>
                <li>toalety, kuchnie i sale</li>
                <li>okna dostępu dla ekipy</li>
              </ul>
            </div>
            <button type="button" @click="goTo('/brief')">Doprecyzuj brief</button>
          </div>

          <img class="hero-image" :src="heroOfficeCleaning" alt="Czysta przestrzeń biurowa po serwisie" />
        </section>

        <section id="zakres" class="surface-matrix">
          <div class="matrix-head">
            <p class="eyebrow">Zakres</p>
            <h2>Zakres czyszczenia dopasowany do realnej przestrzeni biura.</h2>
            <p>
              Poniżej pokazujemy obszary, które mogą wejść do zlecenia. Nie są to pakiety ani przyciski wyboru,
              tylko informacja, co warto opisać w briefie, żeby przygotować sensowną wycenę.
            </p>
          </div>
          <div class="surface-grid">
            <article v-for="area in scopeAreas" :key="area.label">
              <h3>{{ area.label }}</h3>
              <p>{{ area.value }}</p>
            </article>
          </div>
        </section>

        <section id="operacje" class="ops-band">
          <div>
            <p class="eyebrow">Operacje</p>
            <h2>Jedna osoba kontaktowa, szybka kwalifikacja i raport po wykonaniu.</h2>
          </div>
          <div class="ops-metrics">
            <article v-for="item in operations" :key="item.metric">
              <strong>{{ item.metric }}</strong>
              <p>{{ item.label }}</p>
            </article>
          </div>
        </section>
      </template>
    </section>
  </main>
</template>
