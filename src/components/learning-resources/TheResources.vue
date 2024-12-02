<template>
    <base-card>
        <base-button @click="setSelectedTab('stored-resources')" 
        :mode="storedResButtonMode">Stored Resources</base-button>
        <base-button @click="setSelectedTab('add-resource')"
        :mode="addResButtonMode">Add Resources</base-button>
    </base-card>
    <!-- 탭을 바꿔도 입력한 값 유지 -->
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
                    description: 'The official Vue.js documentation.',
                    link: 'https://vuejs.org'
                },
                {
                    id: 'google',
                    title: 'Google',
                    description: 'Learn to goolge',
                    link: 'https://www.google.co.kr/?hl=ko'
                },
            ]
        };
    },
    provide(){
        return {
            resources: this.storedResources,
            addResource: this.addResource,
            deleteResource: this.removeResource
        }
    },
    computed: {
        storedResButtonMode(){
            return this.selectedTab === 'stored-resources' ? null : 'flat';
        },
        addResButtonMode() {
            return this.selectedTab === 'add-resources' ? null : 'flat';
        }
    },
    methods: {
        setSelectedTab(tab){
            this.selectedTab = tab;
        },
        addResource(title, description, url){
            const newResource = {
                id: new Date().toISOString(),
                title: title,
                description: description,
                link: url
            };

            this.storedResources.unshift(newResource); //unshift 는 push 와 유사하나 배열의 맨 앞에 추가한다는 점이 다르다.
            this.selectedTab = 'stored-resources';
        },
        removeResource(resId){
            const resIndex = this.storedResources.findIndex(res => res.id === resId);
            this.storedResources.splice(resIndex, 1);
        }
    }
}
</script>