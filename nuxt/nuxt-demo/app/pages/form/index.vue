<template>
  <div class="flex justify-center min-h-screen items-center">
    <UForm @submit="onSubmit" :state="state" :schema="schema"
      class="flex flex-col md:flex-row gap-10 items-start border border-gray-50 bg-white text-black p-4 rounded-lg shadow">
      <div>
        <h1 class="text-xl font-semibold mb-2">Job Details</h1>
        <UFormField name="title" label="Job Title *" :ui="{
          label: 'text-green-700'
        }" class="p-2">
          <UInput v-model="state.title" size="lg" class="w-full" placeholder="e.g Senior Software Developer " />
        </UFormField>
        <UFormField name="desc" label="Job Description" :ui="{
          label: 'text-green-700'
        }" class="p-2">
          <UTextarea v-model="state.desc" size="lg" class="w-full"
            placeholder="Describe the role and what candidate will do..." />
        </UFormField>
        <UFormField name="resp" label="Job Responsibilities" :ui="{
          label: 'text-green-700'
        }" class="p-2">
          <UTextarea v-model="state.resp" size="lg" class="w-full" placeholder="List key Responsibilities..." />
        </UFormField>
        <UFormField name="skills" label="Required Skills" :ui="{
          label: 'text-green-700'
        }" class="p-2">
          <div class="flex gap-5">
            <UInput v-model="state.skills" size="lg" class="flex-1" placeholder="Add a Skill..." />
            <UButton size="lg" class="cursor-pointer hover:bg-amber-500">Add</UButton>
          </div>
        </UFormField>
        <div class="flex">
          <!-- convert input to number => v-model.number -->
          <UFormField name="minSal" label="Min Salary ($)" :ui="{
            label: 'text-green-700'
          }" class="p-2 flex-1">
            <UInput v-model.number="state.minSal" type="number" min="1000" size="lg" placeholder="60000"
              class="w-full" />
          </UFormField>
          <UFormField name="maxSal" label="Max Salary ($)" :ui="{
            label: 'text-green-700'
          }" class="p-2 flex-1">
            <UInput v-model.number="state.maxSal" type="number" min="1001" size="lg" placeholder="90000"
              class="w-full" />
          </UFormField>
        </div>
      </div>
      <div>
        <h1 class="text-xl font-semibold mb-2">Additional Information</h1>
        <UFormField name="client" label="Client Company *" :ui="{
          label: 'text-green-700'
        }" class="p-2">
          <USelect v-model="state.client" placeholder="Select Client" :items="items" :ui="{
            trailingIcon: 'group-data-[state=open]:rotate-180 transition-transform duration-200'
          }" class="w-full" />
        </UFormField>
        <UFormField name="openings" label="Number Of Openings *" :ui="{
          label: 'text-green-700'
        }" class="p-2">
          <UInput v-model.number="state.openings" placeholder="1" type="number" :ui="{
          }" class="w-full" />
        </UFormField>
        <UFormField name="location" label="Job Location" :ui="{
          label: 'text-green-700'
        }" class="p-2">
          <UInput v-model="state.location" size="md" class="w-full" placeholder="e.g. New York, Remote" />
        </UFormField>
        <UFormField name="template" label="Hiring Stage Template" :ui="{
          label: 'text-green-700'
        }" class="p-2">
          <USelect v-model="state.template" placeholder="Select template" :items="items" :ui="{
            trailingIcon: 'group-data-[state=open]:rotate-180 transition-transform duration-200'
          }" class="w-full" />
        </UFormField>
        <UFormField name="recruiter" label="Assign Recruiter" :ui="{
          label: 'text-green-700'
        }" class="p-2">
          <USelect v-model="state.recruiter" placeholder="Select Recruiter" :items="items" :ui="{
            trailingIcon: 'group-data-[state=open]:rotate-180 transition-transform duration-200'
          }" class="w-full" />
        </UFormField>
        <div>
          <h1>Active Status</h1>
          <div class="flex items-center justify-between">
            <p class="text-gray-500">Make this job visible to candidates</p>
            <USwitch v-model="state.isActive" />
          </div>
        </div>
        <div class="flex mt-10 items-center gap-2">
          <UButton class="flex-1 cursor-pointer hover:bg-amber-500"
            :ui="{ base: 'justify-center bg-white text-black border border-gray-300' }">Save Draft
          </UButton>
          <UButton type="submit" class="flex-1 cursor-pointer hover:bg-amber-500"
            :ui="{ base: 'bg-black text-white justify-center' }">Publish Job
          </UButton>
        </div>
      </div>
    </UForm>
  </div>

</template>
<script lang="ts" setup>
import type { FormSubmitEvent } from '@nuxt/ui'
const toast = useToast()
const items = ref(['Backlog', 'Todo', 'In Progress', 'Done'])
// validation
// step1: store data in a state
const state = reactive({
  title: '',
  desc: '',
  resp: '',
  skills: '',
  minSal: undefined,
  maxSal: undefined,
  client: '',
  openings: undefined,
  location: '',
  template: '',
  recruiter: '',
  isActive: true
})
// Create a validation schema
// schema should match with the state
import * as v from 'valibot'
const schema =
  v.object({
    title: v.pipe(
      v.string(),
      v.minLength(1, 'Job title is required')
    ),
    desc: v.pipe(v.string(), v.minLength(10, 'Too short')),
    resp: v.optional(v.string()),
    skills: v.pipe(v.string(),
      v.minLength(1, "Select atleast one skill")),
    minSal: v.pipe(
      v.number('Minimum salary is required'),
      v.minValue(1000, 'Minimum salary must be >= 1000')
    ),
    maxSal: v.pipe(
      v.number('Maximum salary is required'),
      v.minValue(1001, 'Max salary must be >= 1001') //wont work for null i.e initial value
    ),
    client: v.pipe(v.string(), v.minLength(1, 'Select client')),
    openings: v.pipe(
      v.number('Enter total number of openings'),
      v.minValue(1, 'Must be at least 1')
    ),
    location: v.optional(v.string()),
    template: v.optional(v.string()),
    recruiter: v.optional(v.string()),
  })
// infer type
type Schema = v.InferOutput<typeof schema>
// form submission
async function onSubmit(event: FormSubmitEvent<Schema>) {
  console.log("Form Data: ", event.data);
  if (event.data.minSal > event.data.maxSal) {
    // toast message for salary validation
    toast.add({
      title: 'Error',
      description: 'Min salary cannot be greater than max salary',
      color: 'error'
    })
    return
  }
  // success toast
  toast.add({
    title: 'Success',
    description: 'Job published successfully',
    color: 'success'
  })
}
</script>
<style scoped></style>