<template>
  <div class="mx-20 my-4" v-if="!action.get.loading">
    <div class="flex flex-row items-center justify-between">
      <h1 class="font-black text-5xl text-gray-900">{{ action.doc.title }}</h1>

      <div class="flex flex-row items-center space-x-1">
        <Button icon-left="arrow-left" @click="router.back()">Go back</Button>
        <Button @click="action.setValue.submit({ status: 'Archived' })" appearance="white"
          class="text-red-400 border-red-400" icon-left="trash" v-if="action.doc.status != 'Archived'">Delete</Button>
        <Button @click="action.setValue.submit({ status: 'ToDo' })" appearance="white"
          class="text-yellow-400 border-yellow-400" icon-left="refresh-ccw"
          v-if="action.doc.status != 'Archived'">Undo</Button>
        <Button @click="action.setValue.submit({ status: 'Completed' })" appearance="white"
          class="text-green-600 border-green-600" icon-left="check" v-if="action.doc.status === 'ToDo'">Mark As
          Done</Button>

      </div>
    </div>
    <div class="flex mt-4 ml-2 space-x-4">
      <Card title="Description" class="flex-1 item-center">

        <div class="flex space-y-8 items-center text-left max-h-64">
          {{ action.doc.description }}
        </div>

      </Card>
      <Card title="Date" class="flex-1 text-center">
        <div class="flex space-y-2 items-center text-left">
          <div>
            <h1>Created On: {{ action.doc.date }}</h1> <h1 align="center"> Due by:  {{ action.doc.due_date }}</h1>
          </div>
        </div>
      </Card>
    </div>



    <div>
      <!-- <TextEditor
          editor-class="prose-sm border max-w-none rounded-b-lg p-3 overflow-auto h-64 focus:outline-none"
          :fixedMenu="true"
          :content="content"
          @change="(val) => (content = val)"
        /> -->
    </div>
  </div>
  <LoadingIndicator v-else class="w-6 text-blue-500" />
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { TextEditor, createDocumentResource, LoadingIndicator, Card, createListResource } from 'frappe-ui'
const router = useRouter()

let actions = createListResource({
  doctype: 'Action',
  fields: ['title', 'description', 'task', 'date','due_date'],
  filters: [
    ['title', '=', props.name]
  ],
  fetchOne: {
    onSuccess() { },
    onError() { }
  },
})

const props = defineProps(['name'])
const content = ref('')
const action = createDocumentResource({
  doctype: 'Action',
  name: props.name,
})

actions.reload()

</script>

<style>
.flex {
  display: flex;
  flex-direction: row;
}
</style>
