<template>
   <nav>
    <ContentNavigation v-slot="{ navigation }">
      <ul class="flex">
        <li class="p-2" v-for="link of navigation" :key="link._path">
          <NuxtLink :to="link._path">{{ link.title }}</NuxtLink>
        </li>
      </ul>
    </ContentNavigation>
    </nav>
</template>
<script setup>

</script>
<style lang="scss" scoped>
.router-link-active{
    color:red;
}
</style>