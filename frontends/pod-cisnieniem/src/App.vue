<script setup>
import { onMounted, ref } from 'vue'
import heroPressureWashing from './assets/hero-pressure-washing.png'

const currentPath = ref(window.location.hash.replace('#', '') || '/')

const areas = [
  'kostka brukowa',
  'podjazdy',
  'tarasy',
  'schody zewnętrzne',
  'obrzeża basenów',
  'mała architektura'
]

const scope = [
  {
    title: 'Bruk i podjazdy',
    text: 'Czyszczenie osadów, nalotów, błota, śladów po oponach i zabrudzeń eksploatacyjnych.'
  },
  {
    title: 'Tarasy i patio',
    text: 'Mycie płyt, kamienia, betonu i powierzchni użytkowych wokół domu lub lokalu.'
  },
  {
    title: 'Baseny i strefy mokre',
    text: 'Czyszczenie obrzeży, dojść, śliskich nalotów i zabrudzeń przy nieckach oraz strefach relaksu.'
  },
  {
    title: 'Doczyszczanie punktowe',
    text: 'Miejsca po donicach, zacieki, zielony nalot, fragmenty szczególnie narażone na cień i wilgoć.'
  }
]

const goTo = (path) => {
  window.location.hash = path
  currentPath.value = path
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

const scrollToSection = (id) => {
  if (currentPath.value !== '/') {
    goTo('/')
    window.setTimeout(() => document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' }), 0)
    return
  }

  document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' })
}

onMounted(() => {
  window.addEventListener('hashchange', () => {
    currentPath.value = window.location.hash.replace('#', '') || '/'
  })
})
</script>

<template>
  <main class="pressure-shell">
    <template v-if="currentPath === '/wycena'">
      <section class="quote-screen">
        <header class="mini-header">
          <button class="mini-brand" type="button" @click="goTo('/')">
            <span class="mini-mark" aria-hidden="true">
              <svg viewBox="0 0 48 48">
                <path d="M12 34h24" />
                <path d="M15 27h18" />
                <path d="M18 20h12" />
                <path d="M35 13l-9 9" />
                <path d="M31 12h6v6" />
                <path d="M13 39h22" />
              </svg>
            </span>
            <span>WodaPodCiśnieniem</span>
          </button>
          <button type="button" @click="goTo('/')">Powrót</button>
        </header>
        <div class="quote-copy">
          <p class="eyebrow">Wycena</p>
          <h1>Podaj powierzchnię, dostęp do wody i typ zabrudzenia.</h1>
        </div>
        <form class="quote-form">
          <div class="form-row">
            <label>
              Co czyścimy?
              <select>
                <option>Kostka brukowa / podjazd</option>
                <option>Taras / patio</option>
                <option>Basen / strefa przy basenie</option>
                <option>Inna powierzchnia</option>
              </select>
            </label>
            <label>
              Szacowany metraż
              <input type="text" placeholder="np. 80 m2" />
            </label>
          </div>
          <div class="form-row">
            <label>
              Dostęp do wody
              <select>
                <option>Tak, na zewnątrz</option>
                <option>Tak, w budynku</option>
                <option>Nie wiem</option>
              </select>
            </label>
            <label>
              Kontakt
              <input type="tel" placeholder="+48 ..." />
            </label>
          </div>
          <label>
            Opis miejsca
            <textarea rows="6" placeholder="Rodzaj nawierzchni, zabrudzenia, dojazd, odpływ wody, zdjęcia do dosłania"></textarea>
          </label>
          <button type="button">Wyślij do wyceny</button>
        </form>
      </section>
    </template>

    <template v-else>
      <section class="hero">
        <img :src="heroPressureWashing" alt="Czyszczenie kostki brukowej wodą pod ciśnieniem" />
        <div class="hero-card">
          <nav aria-label="Główne">
            <span class="hero-brand">
              <span class="hero-mark" aria-hidden="true">
                <svg viewBox="0 0 48 48">
                  <path d="M12 34h24" />
                  <path d="M15 27h18" />
                  <path d="M18 20h12" />
                  <path d="M35 13l-9 9" />
                  <path d="M31 12h6v6" />
                  <path d="M13 39h22" />
                </svg>
              </span>
              WodaPodCiśnieniem
            </span>
            <button type="button" @click="scrollToSection('zakres')">Zakres</button>
            <button type="button" @click="scrollToSection('obszary')">Obszary</button>
            <button type="button" @click="goTo('/wycena')">Wycena</button>
          </nav>
          <p class="eyebrow">Mycie wodą pod ciśnieniem</p>
          <h1>Podjazdy, kostka, tarasy i strefy basenowe bez zbędnych formalności.</h1>
          <p>
            Dobieramy ciśnienie do nawierzchni, sprawdzamy dostęp do wody i czyścimy zewnętrzne powierzchnie
            z nalotów, osadów i zabrudzeń użytkowych.
          </p>
          <button type="button" @click="goTo('/wycena')">Poproś o wycenę</button>
        </div>
      </section>

      <section id="zakres" class="scope-section">
        <article v-for="item in scope" :key="item.title">
          <h2>{{ item.title }}</h2>
          <p>{{ item.text }}</p>
        </article>
      </section>

      <section id="obszary" class="areas-band">
        <div>
          <p class="eyebrow">Powierzchnie</p>
          <h2>Prosty opis miejsca wystarczy do pierwszej kwalifikacji.</h2>
        </div>
        <div class="area-list">
          <span v-for="area in areas" :key="area">{{ area }}</span>
        </div>
      </section>
    </template>
  </main>
</template>
