<template>
  <base-card>
    <base-button 
      @click="setSelectedTab('stored-resources')"
      :mode="storedResourcesButtonMode"
      >
      Stored Resources
    </base-button>
    <base-button 
      @click="setSelectedTab('add-resource')"
      :mode="addResourceButtonMode">
      Add Resource
    </base-button>
  </base-card>
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import StoredResources from './StoredResources.vue';
import AddResource from './AddResource.vue';

export default {
  components: {
    StoredResources,
    AddResource
  },
  data() {
    return {
      selectedTab: 'stored-resources',
      storedResources: [
        { 
          id: 'official-guide', 
          title: 'Official Guide', 
          description: 'The official Vue.js documentation',
          link: 'https://vuejs.org'
        },
        { 
          id: 'duckduckgo', 
          title: 'DuckDuckGo', 
          description: 'The best search engine',
          link: 'https://duckduckgo.com'
        },
      ]
    };
  },
  provide() {
    return {
      resources: this.storedResources,
      addResource: this.addResource,
      removeResource: this.removeResource
    };
  },
  computed: {
    storedResourcesButtonMode() {
      return this.selectedTab === 'stored-resources' ? null : 'flat';
    },
    addResourceButtonMode() {
      return this.selectedTab === 'add-resource' ? null : 'flat';
    },
  },
  methods: {
    setSelectedTab(tab) {
      this.selectedTab = tab;
    },
    addResource(title, description, link) {
      const newResource = {
        id: new Date().toISOString(),
        title: title,
        description: description,
        link: link
      };
      this.storedResources.unshift(newResource);
      this.selectedTab = 'stored-resources';
    },
    removeResource(resourceID) {
      const resourceIndex = this.storedResources.findIndex(res => res.id === resourceID);
      this.storedResources.splice(resourceIndex, 1);
    }
  }
}
</script>