<script setup>
import Input from "../components/Input.vue";
import Title from "../components/Title.vue";
import Select from "../components/Select.vue";
import { ref, computed, watch } from "vue"
import Checkbox from "../components/Checkbox.vue";
import Button from "../components/Button.vue";

const firstName = ref("")
const lastName = ref("")
const email = ref("")
const phone = ref("")
const password = ref("")
const country = ref("")
const city = ref("")
const postalCode = ref("")
const acceptedTerms = ref(false)
const confirm18 = ref(false)

const countries = [
  { value: "ro", label: "Romania" },
  { value: "it", label: "Italy" },
  { value: "fr", label: "France" },
]

const citiesByCountry = {
  ro: [
    { value: "bucharest", label: "Bucharest" },
    { value: "cluj", label: "Cluj-Napoca" },
    { value: "iasi", label: "Iași" },
  ],
  it: [
    { value: "rome", label: "Rome" },
    { value: "milan", label: "Milan" },
  ],
  fr: [
    { value: "paris", label: "Paris" },
    { value: "lyon", label: "Lyon" },
  ],
}

const cityOptions = computed(() => citiesByCountry[country.value] ?? [])

watch(country, () => {
  city.value = ""
})
const roles = [
  { value: "frontend", label: "Frontend Developer" },
  { value: "backend", label: "Backend Developer" },
  { value: "qa", label: "QA" },
]

const selectedRole = ref("")
const errors = ref({})

const resetForm = () => {
  firstName.value = ""
  lastName.value = ""
  email.value = ""
  phone.value = ""
  password.value = ""
  selectedRole.value = ""
  country.value = ""
  city.value = ""
  postalCode.value = ""
  acceptedTerms.value = false
  confirm18.value = false
  errors.value = {}
}
const validate = () => {
  const e = {}

  if (!firstName.value.trim()) e.firstName = "First name is required"
  if (!lastName.value.trim()) e.lastName = "Last name is required"
  if (!email.value.trim()) e.email = "Email is required"
  if (!phone.value.trim()) e.phone = "Phone is required"
  if (!password.value.trim()) e.password = "Password is required"
  if (!selectedRole.value) e.role = "Role is required"
  if (!country.value) e.country = "Country is required"
  if (!city.value) e.city = "City is required"
  if (!postalCode.value.trim()) e.postalCode = "Postal code is required"
  if (!acceptedTerms.value) e.acceptedTerms = "You must accept terms"
  if (!confirm18.value) e.confirm18 = "You must confirm 18+"

  errors.value = e
  return Object.keys(e).length === 0
}
const onSubmit = () => {
  if (!validate()) return

  const data = {
    firstName: firstName.value,
    lastName: lastName.value,
    email: email.value,
    phone: phone.value,
    password: password.value,
    role: selectedRole.value,
    country: country.value,
    city: city.value,
    postalCode: postalCode.value,
    acceptedTerms: acceptedTerms.value,
    confirm18: confirm18.value,
  }

  console.log("FORM DATA:", data)
  resetForm()
}
</script>

<template>
  <form @submit.prevent="onSubmit">
    <div class="form-wrapper">
      <Title class="mb-10" text="Form"/>
      <div class="form-group flex gap-x-2">
        <div class="w-full flex flex-col">
          <Input v-model="firstName" type="text" placeholder="Your First Name" />
          <p v-if="errors.firstName" class="text-red-500 text-start text-sm mt-1">
            {{ errors.firstName }}
          </p>
        </div>

        <div class="w-full flex flex-col">
          <Input v-model="lastName" type="text" placeholder="Your Last Name" />
          <p v-if="errors.lastName" class="text-red-500 text-start text-sm mt-1">
            {{ errors.lastName }}
          </p>
        </div>
      </div>
      <div class="form-group flex gap-x-2">
        <div class="w-full flex flex-col">
          <Input v-model="email" type="email" placeholder="Your E-mail" />
          <p v-if="errors.email" class="text-red-500 text-start text-sm mt-1">{{ errors.email }}</p>
        </div>

        <div class="w-full flex flex-col">
          <Input v-model="phone" type="tel" placeholder="Your Phone" />
          <p v-if="errors.phone" class="text-red-500 text-start text-sm mt-1">{{ errors.phone }}</p>
        </div>
      </div>
      <div class="form-group">
        <Input v-model="password" type="password" placeholder="Set a password" />
        <p v-if="errors.password" class="text-red-500 text-start text-sm mt-1">{{ errors.password }}</p>
      </div>
      <div class="form-group">
        <Select v-model="selectedRole" :options="roles" placeholder="Select a role" />
        <p v-if="errors.role" class="text-red-500 text-start text-sm mt-1">{{ errors.role }}</p>
      </div>
      <div class="form-group">
        <Select v-model="country" :options="countries" placeholder="Select country" />
        <p v-if="errors.country" class="text-red-500 text-start text-sm mt-1">{{ errors.country }}</p>
      </div>

      <div class="form-group">
        <Select
            v-model="city"
            :options="cityOptions"
            placeholder="Select city"
            :disabled="!country"
        />
        <p v-if="errors.city" class="text-red-500 text-start text-sm mt-1">{{ errors.city }}</p>
      </div>

      <div class="form-group">
        <Input v-model="postalCode" type="text" placeholder="Postal code" />
        <p v-if="errors.postalCode" class="text-red-500 text-sm text-start mt-1">{{ errors.postalCode }}</p>
      </div>
      <div class="checkbox-container flex flex-col gap-y-2 items-start">
        <Checkbox
            v-model="acceptedTerms"
            label="Sunt de acord cu termenii și condițiile"
        />
        <p v-if="errors.acceptedTerms" class="text-red-500 text-start text-sm mt-1">
          {{ errors.acceptedTerms }}
        </p>
        <Checkbox
            v-model="confirm18"
            label="Confirm că am peste 18 ani"
        />
        <p v-if="errors.confirm18" class="text-red-500 text-start text-sm mt-1">
          {{ errors.confirm18 }}
        </p>
      </div>
      <div class="button-container">
        <Button class="mt-10" text="Trimite" type="submit" />
      </div>
    </div>
  </form>
</template>

<style lang="scss" scoped>
@import "./SimpleForm";
</style>