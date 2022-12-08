<template>
  <div class="flex items-center gap-4">
    <h3 class="font-medium m-0">Contact list</h3>

    <el-button :type="$elComponentType.primary" @click="createNewContact">
      <template #icon>
        <IconPlus class="w-5 h-5" />
      </template>
      Add Contact
    </el-button>

    <el-button
      class="!ml-0"
      :type="$elComponentType.danger"
      @click="$router.replace({ name: $routeNames.login })"
    >
      Logout
    </el-button>
  </div>
  <el-tabs v-model="activeName" class="demo-tabs" @tab-click="handleClick">
    <el-tab-pane label="Cards" name="Cards">
      <div class="grid-cols-[repeat(auto-fill,_minmax(320px,_1fr))] grid gap-5 my-5">
        <ContactItem
          v-for="contact in contacts"
          :key="contact.id"
          class="cursor-pointer"
          :contact="contact"
          @click="editContact(contact.id)"
          @delete="deleteContact"
          @save="updateContact"
        />
      </div>
    </el-tab-pane>

    <el-tab-pane label="Table" name="Table">
      <el-table :data="contacts" style="width: 100%">
        <!-- IMAGE -->
        <el-table-column prop="image" width="100" label="Image">
          <template #default="scope">
            <el-image :src="scope.row.image" />
          </template>
        </el-table-column>

        <!-- NAME -->
        <el-table-column prop="name" label="Name" width="150" />

        <!-- DESCRIPTION -->
        <el-table-column prop="description" label="Description" />

        <!-- ACTIONS -->
        <el-table-column label="Operations" width="150">
          <template #default="scope">
            <el-button size="small">
              Edit
            </el-button>
            <el-button
              size="small"
              type="danger"
              @click="handleDelete(scope.$index)"
            >
              Delete
            </el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-tab-pane>
  </el-tabs>
</template>
<script lang="ts" setup>
import type { TabsPaneContext } from 'element-plus'

const router = useRouter()
const { $routeNames } = useGlobalProperties()

const contactsStore = useContactsStore()
const { contacts } = storeToRefs(contactsStore)
const { updateContact, deleteContact } = contactsStore

const activeName = ref('Cards')

const handleClick = (tab: TabsPaneContext, event: Event) => {
  console.log(tab, event)
}

function createNewContact () {
  router.push({ name: $routeNames.upsertContact, params: { contactId: 'new' } })
}

function editContact (contactId: number) {
  router.push({ name: $routeNames.upsertContact, params: { contactId } })
}

function handleDelete (index: number) {
  contacts.value.splice(index, 1)
}

</script>

<style>
.demo-tabs > .el-tabs__content {
  padding: 32px;
  color: #6b778c;
  font-size: 32px;
  font-weight: 600;
}</style>
