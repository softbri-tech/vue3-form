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

const onSubmit = () => {
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
}
</script>

<template>
  <form @submit.prevent="onSubmit">
    <div class="form-wrapper">
      <Title class="mb-10" text="Form"/>
      <div class="form-group flex gap-x-2">
        <Input v-model="firstName" type="text" placeholder="Your First Name" />
        <Input v-model="lastName" type="text" placeholder="Your Last Name" />
      </div>
      <div class="form-group flex gap-x-2">
        <Input v-model="email" type="email" placeholder="Your E-mail" />
        <Input v-model="phone" type="phone" placeholder="Your Phone" />
      </div>
      <div class="form-group">
        <Input v-model="password" type="password" placeholder="Set a password" />
      </div>
      <div class="form-group">
        <Select v-model="selectedRole" :options="roles" placeholder="Select a role" />
      </div>
      <div class="form-group">
        <Select v-model="country" :options="countries" placeholder="Select country" />
      </div>

      <div class="form-group">
        <Select
            v-model="city"
            :options="cityOptions"
            placeholder="Select city"
            :disabled="!country"
        />
      </div>

      <div class="form-group">
        <Input v-model="postalCode" type="text" placeholder="Postal code" />
      </div>
      <div class="checkbox-container flex flex-col gap-y-2 items-start">
        <Checkbox
            v-model="acceptedTerms"
            label="Sunt de acord cu termenii și condițiile"
        />
        <Checkbox
            v-model="confirm18"
            label="Confirm că am peste 18 ani"
        />
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