<template>
  <div class="container-sm">
    <div class="row">
      <div class="col-12">
        <div v-if="submitted" class="mt-3 alert alert-success">
          <strong>Success!</strong> Your ticket has been submitted.
        </div>
      </div>
    </div>
    <h6 class="mb-3">Ticket Details</h6>
    <div class="row">
      <div class="col-6">
        <div class="mb-3">
          <label for="categorySelect" class="form-label">Category</label>
          <select id="categorySelect" class="form-select" v-model="selectedCategory" :disabled="submitted">
            <option disabled value="">Select a category</option>
            <option v-for="category in categories" :key="category" :value="category">{{ category }}</option>
          </select>
        </div>
      </div>
      <div class="col-6">
        <div class="mb-3">
          <label for="typeDropdown" class="form-label">Type</label>
          <div class="dropdown">
            <button class="btn btn-secondary dropdown-toggle" type="button" id="typeDropdown" data-bs-toggle="dropdown" aria-expanded="false" :disabled="submitted">Type</button>
            <ul class="dropdown-menu" aria-labelledby="typeDropdown">
              <li v-for="type in currentTypes" :key="type">
                <a class="dropdown-item d-flex justify-content-between align-items-center" href="#" @click.prevent="toggleTypeSelection(type)">{{ type }}
                  <span v-if="isSelectedType(type)" class="badge bg-primary rounded-pill">✓</span>
                </a>
              </li>
            </ul>
          </div>
          <div v-if="selectedTypes.length > 0" class="mt-3">
            <ul>
              <li v-for="type in selectedTypes" :key="type">
                <span class="badge bg-secondary rounded-pill">
                  {{ type }}
                </span>
                </li>
            </ul>
          </div>
        </div>
        </div>
      </div>
      <div class="col-12">
        <div class="mb-3">
          <label for="ticketSubject" class="form-label">Subject</label>
          <input v-if="!submitted" type="text" class="form-control" id="ticketSubject" placeholder="" v-model="subject">
          <div v-else>{{ subject }}</div>
        </div>
      </div>
      <div class="col-12">
        <div class="mb-3">
          <label for="ticketDescription" class="form-label">Description</label>
          <textarea v-if="!submitted" class="form-control" id="ticketDescription" rows="3" v-model="description"></textarea>
          <div v-else> {{ description }}</div>
        </div>
      </div>
      <div class="col-12">
        <div class="mb-3">
          <label for="fileUpload" class="form-label">Ticket Files & Documents</label>
          <br />
          <div id="fileUpload">
            <div v-for="(file, index) in files" :key="file.id" class="d-flex align-items-center justify-content-between">
              <a href="#" class="me-2">{{ file.name  }}</a>
              <span v-if="!submitted" @click.prevent="removeFile(index)" class="delete-file">🗑️</span>
            </div>
          </div>
          <button v-if="!submitted" class="btn btn-outline-secondary btn-lg my-3 " @click.prevent="addFile">
            + Attach File
          </button>
        </div>
      </div>
    <div v-if="!submitted" class="col-12 d-flex flex-row-reverse">
      <button type="button" class="btn btn-secondary mx-3" @click.prevent="submitForm">Submit</button>
      <button class="btn btn-outline-secondary" @click="clearForm" >Clear</button>
    </div>
    <div v-else class="col-12 d-flex flex-row-reverse">
      <button class="btn btn-outline-secondary" @click="clearForm" >Clear</button>
    </div>
    </div>
</template>

<script setup lang="ts">
import { ref, computed, defineEmits } from 'vue'

interface FileItem {
  id: number
  name: string
}

interface TypeOptions {
  [key: string]: string[]
}

const emit = defineEmits(['close'])

const categories = ref(['Hardware', 'Software', 'Network', 'In-Processing'])
const types = ref<TypeOptions>({
  Hardware: ['Laptop', 'Mobile', 'Peripherals', 'Desk Phone', 'Printers', 'Other'],
  Software: ['Teams/Zoom', 'Mobile Blackberry', 'Adobe', 'Outlook', 'Microsoft Office', 'Other'],
  Network: ['Network Access', 'Connectivity', 'VPN', 'Drivers', 'Other'],
  'In-Processing': ['Access Badge', 'Common Access Card (CAC)', 'SIPR', 'Trello'],
})



let selectedCategory = ref('')
let selectedTypes = ref<string[]>([])
let files = ref<FileItem[]>([])
let submitted = ref(false);
let subject = ref("")
let description = ref("")


let fileCount = 0

const currentTypes = computed(() => {
  return selectedCategory.value ? types.value[selectedCategory.value] : []
})

function toggleTypeSelection(type: string) {
  const index = selectedTypes.value.indexOf(type)
  if (index === -1) {
    selectedTypes.value.push(type)
  } else {
    selectedTypes.value.splice(index, 1)
  }
}

function isSelectedType(type: string) {
  return selectedTypes.value.includes(type)
}

function clearForm() {
  console.log('clear form')
  fileCount = 0
  submitted.value = false
  selectedCategory = ref('')
  selectedTypes = ref<string[]>([])
  files = ref<FileItem[]>([])
  submitted = ref(false);
  subject = ref("")
  description = ref("")
  emit('close')
}


function addFile() {
  fileCount += 1
  const fileName = `nameoffileattached${fileCount}.pdf`
  files.value.push({  id: fileCount, name: fileName })
}

function removeFile(index: number) {
  files.value.splice(index, 1)
}

function submitForm() {
  submitted.value = true
  // Validations then send to server
  console.log('submitted')
  console.log('Category:', selectedCategory.value)
  console.log('Types:', selectedTypes.value)
  console.log('Subject:', subject.value)
  console.log('Description:', description.value)
  console.log('File Count',fileCount)
  console.log('Files:', files.value)

}

</script>

<style scoped>
.container-sm {
  max-width: 500px;
}
.delete-file {
  cursor: pointer;
}
</style>
