
<template>
  <div class="search flex items-center space-x-2 shadow-md rounded p-3">
    <span class="text-gray-400 text-xl">
      <i class="fas fa-search"></i>
    </span>
    <input @keyup="handleChange" class="flex-auto py-3 bg-none outline-none border-none" placeholder="...Search Users" type="text">
  </div>
</template>

<script lang="ts">
  import { Component, Vue } from 'vue-property-decorator';

  @Component
  export default class SearchBar extends Vue {
    searchValue = '';

    handleChange(e: any){
      const value = e.target.value;
      const query = {...this.$route.params, search: value}
      this.$router.replace({ query });
    }
  }
</script>

<style lang="scss" scoped>
.search{
  background-color: #FAFAFA;

  input{
    background: none;
  }
}
</style>