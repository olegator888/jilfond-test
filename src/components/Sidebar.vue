<script setup>
import SearchInput from './SearchInput.vue';
import EmployeeCard from './EmployeeCard.vue';
import { computed, ref, watch } from 'vue';
import { debounce } from 'lodash'
import capitalize from '../utils/capitalize';
import { useStore } from "vuex";
import axios from "axios";

const employees = ref([]);
const search = ref("");
const loading = ref(false);
const error = ref(false);

const onSearchChange = (e) => {
  search.value = e.target.value;
  error.value = false;
}

const message = computed(() => {
  if (loading.value && !!search.value.length) {
    return "ищем...";
  }

  if (error.value) {
    return "Произошла ошибка";
  }

  return !!search.value.length ? "ничего не найдено" : "начните поиск";
});

const fetchEmployees = debounce(async (search) => {
  try {
    // get employees names from search value
    const names = search.split(", ");

    // create query string for filtering by names
    let query = "";
    names.forEach((name, index) => {
      const prefix = index === 0 ? "?" : "&";
      query += `${prefix}name=${capitalize(name)}`;
    });

    const { data } = await axios.get(`https://jsonplaceholder.typicode.com/users${query}`);

    employees.value = data;
  } catch (e) {
    console.log(e);
    error.value = true;
  } finally {
    loading.value = false;
  }
}, 500);

watch(search, (newSearch) => {
  try {
    loading.value = true;

    // prevent api call when search is empty
    if (newSearch) {
      fetchEmployees(newSearch);
    }
  } catch (error) {
    console.log(error);
  }
});

// managing store
const store = useStore();

const selectEmployee = (employeeId) => {
  store.commit("selectEmployee", employeeId);
}

const selectedEmployeeId = computed(() => store.state.selectedEmployeeId);
</script>

<template>
  <div class="sidebar">
    <div class="sidebar__header">
      <h3 class="title">
        Поиск сотрудников 
      </h3>
      <SearchInput :search="search" :on-change="onSearchChange" />
    </div>
    <div class="sidebar__results">
      <h4 class="title">Результаты</h4>
      <span v-if="error || loading || !search || !employees.length" class="message">
        {{ message }}
      </span>
      <div v-else class="list">
        <EmployeeCard 
          v-for="user in employees" 
          :user="user" 
          :is-selected="user.id === selectedEmployeeId"
          :on-select="() => selectEmployee(user.id)"
        />
      </div>
    </div>
  </div>
</template>

<style lang="scss">
  .sidebar {
    flex-shrink: 0;
    width: 320px;
    height: 100%;
    border-right: 1px solid #E0E0E0;
    padding: 30px; 
    padding-left: 20px;

    &__header {
      margin-bottom: 30px;

      .title {
        margin-bottom: 14px;
        color: #333;
        font-size: 16px;
        font-style: normal;
        font-weight: 600;
        line-height: 140%; /* 22.4px */
      }
    }

    &__results {
      .title {
        color: #333;
        font-size: 16px;
        font-style: normal;
        font-weight: 600;
        line-height: 140%; /* 22.4px */
        margin-bottom: 10px;
      }

      .message {
        color: #76787D;
        font-size: 14px;
        font-style: normal;
        font-weight: 400;
        line-height: normal;
      }

      .list {
        padding: 6px;
        margin-top: 8px;
        max-height: 350px;
        display: flex;
        flex-direction: column;
        gap: 18px;
        overflow-y: scroll;
      }
    }
  }
</style>