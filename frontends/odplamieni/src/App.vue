<script setup>
import { onMounted, ref } from 'vue'
import heroFabricCleaning from './assets/hero-fabric-cleaning.png'

const currentPath = ref(window.location.hash.replace('#', '') || '/')

const surfaces = [
  {
    name: 'Dywany i wykładziny',
    text: 'Odświeżanie włókien, odplamianie miejscowe i ekstrakcyjne pranie powierzchni tekstylnych.'
  },
  {
    name: 'Kanapy i narożniki',
    text: 'Czyszczenie tkanin obiciowych, śladów po codziennym użytkowaniu, plam po jedzeniu i napojach.'
  },
  {
    name: 'Fotele, krzesła, pufy',
    text: 'Szybkie czyszczenie mniejszych elementów tapicerowanych w domu, biurze albo lokalu usługowym.'
  },
  {
    name: 'Materace',
    text: 'Odświeżanie powierzchni spania, neutralizacja zapachów i czyszczenie miejscowych zabrudzeń.'
  }
]

const stainTypes = ['kawa i herbata', 'wino i soki', 'ślady po zwierzętach', 'błoto', 'tłuste plamy', 'zapachy']

const steps = [
  'Opisujesz zabrudzenie i materiał',
  'Dobieramy metodę oraz termin',
  'Czyścimy u Ciebie na miejscu',
  'Powierzchnia schnie naturalnie'
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
  <main class="page-shell">
    <header class="topbar">
      <button class="brand" type="button" @click="goTo('/')">
        <span class="brand-mark" aria-hidden="true">
          <svg viewBox="0 0 48 48">
            <path d="M13 30c5-9 17-9 22 0" />
            <path d="M16 35h16" />
            <path d="M24 12c4 5 7 9 7 14a7 7 0 0 1-14 0c0-5 3-9 7-14Z" />
            <path d="M21 27c2 2 4 2 6 0" />
          </svg>
        </span>
        <span>Odplamieni</span>
      </button>
      <nav aria-label="Główne">
        <button type="button" @click="scrollToSection('zakres')">Zakres</button>
        <button type="button" @click="scrollToSection('plamy')">Plamy</button>
        <button type="button" @click="goTo('/zgloszenie')">Zgłoszenie</button>
      </nav>
    </header>

    <template v-if="currentPath === '/zgloszenie'">
      <section class="request-view">
        <div class="request-copy">
          <button class="ghost-button" type="button" @click="goTo('/')">Powrót</button>
          <p class="eyebrow">Zgłoszenie</p>
          <h1>Opisz materiał i plamę. Resztę dopasujemy po kontakcie.</h1>
          <p>Do wyceny wystarczy rodzaj mebla lub dywanu, orientacyjny rozmiar, typ plamy i lokalizacja.</p>
        </div>

        <form class="request-form">
          <label>
            Co czyścimy?
            <select>
              <option>Kanapa lub narożnik</option>
              <option>Dywan</option>
              <option>Wykładzina</option>
              <option>Fotel lub krzesła</option>
              <option>Materac</option>
            </select>
          </label>
          <div class="form-row">
            <label>
              Lokalizacja
              <input type="text" placeholder="Miasto / dzielnica" />
            </label>
            <label>
              Kontakt
              <input type="tel" placeholder="+48 ..." />
            </label>
          </div>
          <label>
            Opis zabrudzenia
            <textarea rows="6" placeholder="Rodzaj materiału, wielkość powierzchni, typ plamy, kiedy powstała"></textarea>
          </label>
          <button type="button">Wyślij zgłoszenie</button>
        </form>
      </section>
    </template>

    <template v-else>
      <section class="hero">
        <div class="hero-copy">
          <p class="eyebrow">Dywany, kanapy, materace</p>
          <h1>Odplamianie i pranie tapicerek bez skomplikowanego zamawiania.</h1>
          <p>
            Przyjeżdżamy z urządzeniem ekstrakcyjnym, dobieramy chemię do materiału i czyścimy u Ciebie na miejscu.
          </p>
          <div class="hero-actions">
            <button type="button" @click="goTo('/zgloszenie')">Zgłoś plamę</button>
            <button type="button" @click="scrollToSection('zakres')">Co czyścimy</button>
          </div>
        </div>
        <img :src="heroFabricCleaning" alt="Czyszczenie dywanu i tapicerki" />
      </section>

      <section id="zakres" class="surface-section">
        <div class="section-head">
          <p class="eyebrow">Zakres</p>
          <h2>Najczęstsze powierzchnie tekstylne.</h2>
        </div>
        <div class="surface-list">
          <article v-for="surface in surfaces" :key="surface.name">
            <h3>{{ surface.name }}</h3>
            <p>{{ surface.text }}</p>
          </article>
        </div>
      </section>

      <section id="plamy" class="stain-band">
        <div>
          <p class="eyebrow">Plamy</p>
          <h2>Najpierw ocena materiału, potem metoda czyszczenia.</h2>
        </div>
        <div class="stain-tags">
          <span v-for="type in stainTypes" :key="type">{{ type }}</span>
        </div>
      </section>

      <section class="steps-section">
        <article v-for="(step, index) in steps" :key="step">
          <span>{{ index + 1 }}</span>
          <p>{{ step }}</p>
        </article>
      </section>
    </template>
  </main>
</template>
