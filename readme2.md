´´´bash
src/
├── domains/
│   └── patients/
│       ├── components/
│       │   └── PatientForm.vue         ← Vista y lógica del formulario
│       ├── services/
│       │   └── patientService.js       ← Acceso a datos (fetch/post/put)
│       ├── models/
│       │   └── Patient.js              ← Modelo de entidad
│       └── views/
│           └── PatientFormPage.vue     ← Página que contiene el formulario

´´´

PatientForm.vue
<script setup>
import { ref, watch, onMounted } from 'vue';

const props = defineProps({
  patient: {
    type: Object,
    default: () => ({
      firstName: '',
      lastName: '',
      birthDate: '',
      photoUrl: ''
    })
  }
});

const emit = defineEmits(['submit']);

const localPatient = ref({ ...props.patient });

watch(() => props.patient, (newVal) => {
  localPatient.value = { ...newVal };
});

function submitForm() {
  emit('submit', localPatient.value);
}
</script>

<template>
  <form @submit.prevent="submitForm" class="p-fluid">
    <div class="field">
      <label for="firstName">First Name</label>
      <input v-model="localPatient.firstName" id="firstName" required />
    </div>
    <div class="field">
      <label for="lastName">Last Name</label>
      <input v-model="localPatient.lastName" id="lastName" required />
    </div>
    <div class="field">
      <label for="birthDate">Birth Date</label>
      <input v-model="localPatient.birthDate" type="date" id="birthDate" required />
    </div>
    <div class="field">
      <label for="photoUrl">Photo URL</label>
      <input v-model="localPatient.photoUrl" id="photoUrl" />
    </div>
    <button type="submit">Guardar</button>
  </form>
</template>

<style scoped>
.field {
  margin-bottom: 1rem;
}
</style>


patientService.js

const BASE_URL = 'http://localhost:3000/patients';

export async function getPatient(id) {
const res = await fetch(`${BASE_URL}/${id}`);
return await res.json();
}

export async function createPatient(patient) {
const res = await fetch(BASE_URL, {
method: 'POST',
headers: { 'Content-Type': 'application/json' },
body: JSON.stringify(patient)
});
return await res.json();
}

export async function updatePatient(id, updates) {
const res = await fetch(`${BASE_URL}/${id}`, {
method: 'PUT',
headers: { 'Content-Type': 'application/json' },
body: JSON.stringify(updates)
});
return await res.json();
}


PatientFormPage.vue

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import PatientForm from '../components/PatientForm.vue';
import { getPatient, createPatient, updatePatient } from '../services/patientService';

const route = useRoute();
const router = useRouter();
const patient = ref(null);
const isEditing = ref(false);

onMounted(async () => {
  const id = route.params.id;
  if (id) {
    patient.value = await getPatient(id);
    isEditing.value = true;
  }
});

async function handleSubmit(data) {
  if (isEditing.value) {
    await updatePatient(patient.value.id, data);
  } else {
    await createPatient(data);
  }
  router.push('/patients'); // redirige a la lista
}
</script>

<template>
  <div>
    <h2>{{ isEditing ? 'Editar Paciente' : 'Crear Paciente' }}</h2>
    <PatientForm :patient="patient" @submit="handleSubmit" />
  </div>
</template>


