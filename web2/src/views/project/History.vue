<template xmlns:v-slot="http://www.w3.org/1999/XSL/Transform">
  <div v-if="items != null">
    <v-toolbar flat color="white">
      <v-app-bar-nav-icon @click="showDrawer()"></v-app-bar-nav-icon>
      <v-toolbar-title>Dashboard</v-toolbar-title>
      <v-spacer></v-spacer>
      <div>
        <v-tabs centered>
          <v-tab key="history" :to="`/project/${projectId}/history`">History</v-tab>
          <v-tab key="activity" :to="`/project/${projectId}/activity`">Activity</v-tab>
          <v-tab key="settings" :to="`/project/${projectId}/settings`">Settings</v-tab>
        </v-tabs>
      </div>
    </v-toolbar>
    <v-data-table
      :headers="headers"
      :items="items"
      hide-default-footer
      class="mt-4"
    >
      <template v-slot:item.tpl_alias="{ item }">
        <a @click="showTaskLog(item.id)">{{ item.tpl_alias }}</a>
        <span style="color: gray; margin-left: 10px;">#{{ item.id }}</span>
      </template>
      <template v-slot:item.status="{ item }">
        {{ item.status }}
      </template>
    </v-data-table>
  </div>
</template>
<script>
import ItemListPageBase from '@/components/ItemListPageBase';
import EventBus from '@/event-bus';

export default {
  mixins: [ItemListPageBase],

  watch: {
    async projectId() {
      await this.loadItems();
    },
  },

  methods: {
    showTaskLog(taskId) {
      EventBus.$emit('i-show-task', {
        taskId,
      });
    },

    getHeaders() {
      return [
        {
          text: 'Task',
          value: 'tpl_alias',
          sortable: false,
        },
        {
          text: 'Status',
          value: 'status',
          sortable: false,
        },
        {
          text: 'User',
          value: 'user_name',
          sortable: false,
        },
        {
          text: 'Start',
          value: 'start',
          sortable: false,
        },
        {
          text: 'Duration',
          value: 'start',
          sortable: false,
        },
      ];
    },

    getItemsUrl() {
      return `/api/project/${this.projectId}/tasks/last`;
    },
  },
};
</script>
