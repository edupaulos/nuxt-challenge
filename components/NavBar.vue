<script setup lang="ts">
const route = useRoute();
const isSearchBarOpen = ref(false);
const searchTerm = ref("");

function toggleSearchBar() {
  isSearchBarOpen.value = !isSearchBarOpen.value;
}

function handleSearch() {
  console.log("Search term:", searchTerm.value);
}

function closeSearchBar() {
  isSearchBarOpen.value = false;
}

const clickHandler = (event: MouseEvent) => {
  const targetElement = event.target as Element;
  if (!targetElement.closest(".search-container")) {
    closeSearchBar();
  }
};

onMounted(() => {
  document.addEventListener("click", clickHandler);
});

onUnmounted(() => {
  document.removeEventListener("click", clickHandler);
});
</script>

<template>
  <div class="navbar-wrapper">
    <div class="navbar">
      <h1 class="logo">Logo</h1>
      <div class="link-container">
        <NuxtLink class="navLink" :class="{ active: route.path === '/' }" to="/"
          >Page 1</NuxtLink
        >
        <NuxtLink
          class="navLink"
          :class="{ active: route.path === '/population' }"
          to="/population"
          >Page 2</NuxtLink
        >
      </div>
      <div class="search-container">
        <button @click="isSearchBarOpen ? handleSearch() : toggleSearchBar()">
          <img class="search-icon" src="@/assets/icons/search.svg" />
        </button>
        <input
          class="search-input"
          v-model="searchTerm"
          v-if="isSearchBarOpen"
          @keydown.enter="handleSearch"
          placeholder="Search..."
        />
      </div>
    </div>
  </div>
</template>

<style scoped lang="sass">
.navbar-wrapper
  background-color: #000000
  width: 100%

.navbar
  max-width: 1200px
  margin: 0 auto
  padding: 0 1rem
  display: flex
  height: 5rem
  justify-content: space-between
  align-items: center

  .logo
    color: white
    font-weight: 800

  .link-container
    display: flex
    gap: 2rem

  .navLink
    color: white
    text-decoration: none
    padding-bottom: 0.4rem

    &.active
      border-bottom: 2px solid white
    &:hover
      border-bottom: 2px solid white

  .search-container
    position: relative
    display: flex

  .search-input
    display: block
    width: 10rem
    outline: none
    border: 1px solid white

  button
    border: none
    background-color: white
    padding: 0.2rem

    .search-icon
      transform: rotate(90deg)
      width: 80%
</style>
