<script setup>
import { ref } from 'vue'

const newCarModel = ref('')
const newCarYear = ref('')
const selectedCarId = ref(1)
const newMaintenance = ref('')

const cars = ref([
  {
    id: 1,
    model: 'Saab 9-5',
    year: 2008,
    maintenance: [
      { id: 1, title: 'Byt tändstift' },
      { id: 2, title: 'Byt drivrem' },
    ],
  },
  {
    id: 2,
    model: 'Volvo V70',
    year: 2012,
    maintenance: [{ id: 1, title: 'Byt olja' }],
  },
])

function addCar() {
  const model = newCarModel.value.trim()

  if (model === '' || newCarYear.value === '') {
    return
  }

  const newCar = {
    id: Date.now(),
    model,
    year: newCarYear.value,
    maintenance: [],
  }

  cars.value.push(newCar)
  selectedCarId.value = newCar.id
  newCarModel.value = ''
  newCarYear.value = ''
}

function selectCar(id) {
  selectedCarId.value = id
  newMaintenance.value = ''
}

function getSelectedCar() {
  return cars.value.find((car) => car.id === selectedCarId.value)
}

function addMaintenance() {
  const selectedCar = getSelectedCar()
  const title = newMaintenance.value.trim()

  if (!selectedCar || title === '') {
    return
  }

  selectedCar.maintenance.push({
    id: Date.now(),
    title,
  })

  newMaintenance.value = ''
}

function removeMaintenance(id) {
  const selectedCar = getSelectedCar()

  if (!selectedCar) {
    return
  }

  selectedCar.maintenance = selectedCar.maintenance.filter(
    (maintenance) => maintenance.id !== id,
  )
}
</script>

<template>
  <main class="page">
    <div class="garage-card">
      <header class="page-header">
        <div class="logo" aria-hidden="true">MG</div>
        <div>
          <p class="eyebrow">Din digitala garagebok</p>
          <h1>MyGarage</h1>
        </div>
      </header>

      <section aria-labelledby="add-car-heading">
        <h2 id="add-car-heading">Lägg till bil</h2>

        <form class="car-form" @submit.prevent="addCar">
          <label>
            Modell
            <input v-model="newCarModel" type="text" placeholder="Till exempel Saab 9-5" />
          </label>

          <label>
            Årsmodell
            <input v-model="newCarYear" type="number" placeholder="2008" />
          </label>

          <button type="submit">Lägg till bil</button>
        </form>
      </section>

      <section aria-labelledby="cars-heading">
        <h2 id="cars-heading">Mina bilar</h2>

        <div class="car-list">
          <button
            v-for="car in cars"
            :key="car.id"
            type="button"
            class="car-button"
            :class="{ selected: car.id === selectedCarId }"
            @click="selectCar(car.id)"
          >
            <strong>{{ car.model }}</strong>
            <span>Årsmodell {{ car.year }}</span>
          </button>
        </div>
      </section>

      <section v-if="getSelectedCar()" aria-labelledby="maintenance-heading">
        <div class="selected-car">
          <p class="eyebrow">Vald bil</p>
          <h2>{{ getSelectedCar().model }}</h2>
          <p>Årsmodell {{ getSelectedCar().year }}</p>
        </div>

        <h2 id="maintenance-heading">Planerat underhåll</h2>

        <form class="add-form" @submit.prevent="addMaintenance">
          <label class="visually-hidden" for="new-maintenance">Nytt underhåll</label>
          <input
            id="new-maintenance"
            v-model="newMaintenance"
            type="text"
            placeholder="Skriv ett nytt underhåll"
          />
          <button type="submit">Lägg till</button>
        </form>

        <p v-if="getSelectedCar().maintenance.length === 0" class="empty-message">
          Inget planerat underhåll
        </p>

        <ul v-else class="maintenance-list">
          <li v-for="maintenance in getSelectedCar().maintenance" :key="maintenance.id">
            <span>{{ maintenance.title }}</span>
            <button
              type="button"
              class="remove-button"
              @click="removeMaintenance(maintenance.id)"
            >
              Ta bort
            </button>
          </li>
        </ul>
      </section>
    </div>
  </main>
</template>
