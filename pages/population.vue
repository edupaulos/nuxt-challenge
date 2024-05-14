<script setup lang="tsx">
const data = ref<PopulationData>([]);
const sortedData = ref<PopulationData>([]);
const source = ref(null);

const expandedIndex = ref(-1);

let selectedYear: string | null = null;
let sortOrder = "asc";

const currentPage = ref(1);
const itemsPerPage = ref(4);

const fetchData = async () => {
  try {
    const response = await fetch(
      "https://datausa.io/api/data?drilldowns=Nation&measures=Population"
    );
    const jsonData = await response.json();
    data.value = jsonData.data;
    sortedData.value = jsonData.data;
    source.value = jsonData.source[0].annotations.source_name;
  } catch (error) {
    console.error("Error fetching data:", error);
  }
};

const toggleExpand = (index: number) => {
  expandedIndex.value = expandedIndex.value === index ? -1 : index;
};

const filterAndSortData = (order: string = sortOrder) => {
  let filteredData = [...data.value];

  if (!filteredData) return;

  if (selectedYear && selectedYear !== "default") {
    filteredData = filteredData.filter((value) => value.Year === selectedYear);
    currentPage.value = 1;
  }

  if (order === "asc") {
    sortedData.value = filteredData.sort(
      (a, b) => Number(a.Year) - Number(b.Year)
    );
  } else {
    sortedData.value = filteredData.sort(
      (a, b) => Number(b.Year) - Number(a.Year)
    );
  }
};

const sortData = (order: string) => {
  sortOrder = order;
  filterAndSortData(order);
};

const filterByYear = (event: Event) => {
  selectedYear = (event.target as HTMLSelectElement).value;
  filterAndSortData();
};

const paginatedData = computed(() => {
  const startIndex = (currentPage.value - 1) * itemsPerPage.value;
  const endIndex = startIndex + itemsPerPage.value;
  return sortedData.value.slice(0, endIndex);
});

const showMoreButton = computed(() => {
  return currentPage.value * itemsPerPage.value < sortedData.value.length;
});

const loadMore = () => {
  currentPage.value++;
};

onMounted(fetchData);
</script>

<template>
  <div class="population-wrapper">
    <div class="header">
      <h1>Population</h1>
      <div class="filters-wrapper">
        <button @click="sortData('asc')">
          <img src="@/assets/icons/arrow-up.svg" alt="Sort Ascending" />
        </button>
        <button @click="sortData('desc')">
          <img src="@/assets/icons/arrow-down.svg" alt="Sort Descending" />
        </button>

        <select
          name="years-filter"
          id="years"
          class="years-filter"
          @change="filterByYear"
        >
          <option value="default">Filter year</option>
          <option v-for="(item, index) in data" :key="index" :value="item.Year">
            {{ item.Year }}
          </option>
        </select>
      </div>
    </div>

    <div>
      <ul>
        <li
          v-for="(item, index) in paginatedData"
          :key="item.Year"
          :style="{
            backgroundColor:
              index % 2 === 0 ? 'var(--secondary-color)' : 'var(--light-gray)',
          }"
          class="list-item"
        >
          <div>
            <div class="item-details">
              <span>{{ item.Year }}</span>
              <button class="expand-button" @click="toggleExpand(index)">
                <img
                  v-if="expandedIndex === index"
                  src="@/assets/icons/minus.svg"
                />
                <img v-else src="@/assets/icons/plus.svg" />
              </button>
            </div>
            <div v-if="expandedIndex === index" class="expanded-details">
              <div class="expanded-details-inner">
                <div>
                  <p>Country</p>
                  <span>{{ item.Nation }}</span>
                  <span> #{{ item["ID Nation"] }}</span>
                </div>
                <div>
                  <p>Population</p>
                  <span>{{ item.Population }}</span>
                </div>
                <div>
                  <p>Source</p>
                  <span>{{ source }}</span>
                </div>
              </div>
            </div>
          </div>
        </li>
      </ul>
      <button v-if="showMoreButton" @click="loadMore" class="show-more-button">
        Show More
      </button>
    </div>
  </div>
</template>

<style scoped lang="sass">
.population-wrapper
  max-width: 1200px
  margin: 2rem auto

  .header
    margin-inline: 2rem
    display: flex
    justify-content: space-between

    .filters-wrapper
      display: flex
      flex-direction: row
      align-items: center
      gap: .5rem

      .years-filter
        border: none
        padding: .5rem 5rem
        background-color: var(--secondary-color)

        option
          display: inline-block
          text-align: right

  h1
    font-size: 3rem

  .list-item
    margin-inline: 2rem
    font-size: 2rem
    font-weight: 800
    padding: 1rem 2rem

    .item-details
      display: flex
      justify-content: space-between
      align-items: center

    .expand-button
      margin-left: auto

      img
        stroke-width: 1px
        width: 5rem
        height: 5rem


    .expanded-details-inner
      border-top: 1px solid var(--primary-color)
      display: flex
      justify-content: space-between
      padding-top: 2rem

      p
        margin: 0
        font-size: 0.8rem
        font-weight: 600

      span
        margin: 0
        font-size: 0.8rem
        font-weight: 300

  .show-more-button
    border: 2px solid var(--primary-color)
    padding: 1rem
    display: block
    width: fit-content
    margin-inline: auto
    margin-top: 2rem
</style>
