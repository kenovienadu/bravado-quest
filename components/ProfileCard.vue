<template>
  
    <section class="user-card border">
      <img class="user-card-img" :src="user.avatar" alt="">

      <div class="user-card-body">
        <div class="title px-4">
          <span class="name">
            <TextHighlight :queries="highlightQueries">{{ user.name }}</TextHighlight>
          </span>
          <span class="email">
            <TextHighlight :queries="highlightQueries">{{ user.email }}</TextHighlight>
          </span>
        </div>

        <div class="company-name px-4">
          <TextHighlight :queries="highlightQueries">{{ user.title }}</TextHighlight>
        </div>

        <div class="address px-4">
          <TextHighlight :queries="highlightQueries">{{ user.address }}, {{ user.city }}</TextHighlight>
        </div>

        <div class="action-wrapper py-3 px-4 mt-4">
          <button class="uppercase">Skip Selection</button>
        </div>
      </div>
    </section>

</template>

<script lang="ts">
  import { Component, Prop, Vue } from 'vue-property-decorator';
  import { IProfile } from '@/common/interfaces';

  // @ts-ignore
  import TextHighlight from 'vue-text-highlight';

  @Component({
    components: { TextHighlight }
  })
  export default class ProfileCard extends Vue {
    @Prop() user!: IProfile;

    get searchQuery(){
      const searchValue = this.$route.query["search"] as string;
      return searchValue || '';
    }

    get highlightQueries(){ // the text-highlight library needs an array of strings to match
      if(!this.searchQuery.length){
        return [];
      }

      return [this.searchQuery];
    }
  }
</script>

<style lang="scss" scoped>
  .user-card{
    display: grid;
    grid-template-columns: 165px auto;
    height: 160px;
    border-radius: 8px;
    overflow: hidden;

    .user-card-img{
      height: 160px;
    }

    .user-card-body{
      padding: 1em 0;

      .title{
        display: flex;
        justify-content: space-between;
        align-items: center;

        .name{
          font-size: 24px;
          line-height: 32px;
        }

        .email{
          opacity: 0.54;
        }
      }

      .company-name{
        font-weight: 700;
        opacity: 0.54;
      }

      .address{
        opacity: 0.54;
      }

      .action-wrapper{
        border-top: 1px solid lightgray;

        button{
          color: #009688;
          font-weight: 500;
        }
      }
    }

  }
</style>