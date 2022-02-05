<template>
  <main>
    <section class="w-10/12 md:w-1/2 mx-auto bg-white content-wrapper">
      <searchbar></searchbar>

      <div class="text-center py-4 opacity-50" v-if="searchQuery.length && searchResults.length">
        Showing {{searchResults.length}} matching results
      </div>

      <div class="user-cards-wrapper" v-if="searchResults.length">
        <RecycleScroller
          class="scroller"
          :items="searchResults"
          :item-size="180"
          key-field="email"
          v-slot="{ item }"
        >
            <user-card :user="item"></user-card>
        </RecycleScroller>
      </div>

      <div class="text-center py-4 opacity-50 py-20" v-if="searchQuery.length && !searchResults.length">
        Sorry, no matching profiles for "{{searchQuery}}"
      </div>

    </section>
  </main>
</template>

<script lang="ts">
  import { Component, Vue } from 'vue-property-decorator';
  import { userProfiles } from '@/common/data';
  import { IUser } from '~/common/interfaces';

  // @ts-ignore
  import { RecycleScroller } from 'vue-virtual-scroller';

  @Component({
    components: { RecycleScroller }
  })
  export default class IndexPage extends Vue {
    profiles: IUser[] = [];

    get searchQuery(){
      const searchValue = this.$route.query["search"] as string;
      return searchValue || '';
    }

    get searchResults(){

      if(!this.searchQuery?.length){
        return this.profiles;
      }

      const filteredProfiles = this.filterProfiles(this.searchQuery);

      return filteredProfiles;
    }

    mounted() {
      this.sortProfiles();
    }

    sortProfiles(){
      const sorted = userProfiles.sort((prev, next) => next.name.toLowerCase() < prev.name.toLowerCase() ? 1 : -1);
      this.profiles = [...sorted];
    }

    filterProfiles(filterPhrase: string){
      return this.profiles.filter(profile => {
        const { name, title, email, city, address } = profile;
        const fieldsToFilter = [name, title, email, city, address];

        return fieldsToFilter.some(field => {
          return field.toLowerCase().includes(filterPhrase.toLowerCase())
        })
      })
    }
  }
</script>

<style lang="scss" scoped>
main{
  min-height: 100vh;
  padding-top: 50px;
  background-color: #EEEEEE;

  .content-wrapper{
    min-height: 40vh;
    border-radius: 4px;
    padding: 1.5em;
  }

  .user-cards-wrapper{
    padding: 1em 0;
    max-height: 70vh;
    overflow: scroll;
  }

  .scroller{
    height: 70vh
  }
}
</style>