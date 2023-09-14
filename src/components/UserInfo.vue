<script setup>
import { ref, computed, watch } from "vue";
import { useStore } from "vuex";
import { debounce } from "lodash";
import axios from "axios";

const store = useStore();

const selectedEmployeeId = computed(() => store.state.selectedEmployeeId);

const user = ref(null);
const loading = ref(false);
const error = ref(false);

const fetchEmployeeById = debounce(async (id) => {
  try {
    const { data } = await axios.get(`https://jsonplaceholder.typicode.com/users/${id}`);

    user.value = data;
  } catch (e) {
    console.log(e);
    error.value = true;
  } finally {
    loading.value = false;
  }
}, 500);

watch(selectedEmployeeId, (newId) => {
  try {
    loading.value = true;
    error.value = false;

    // prevent api call when newId is empty
    if (newId) {
      fetchEmployeeById(newId);
    }
  } catch (error) {
    console.log(error);
  }
});

const message = computed(() => {
  if (loading.value) {
    return "Загружаем информацию о сотруднике...";
  }

  if (!!error.value) {
    return "Произошла ошибка";
  }

  return "Выберите сотрудника, чтобы посмотреть его профиль"
});

</script>

<template>
  <div v-if="selectedEmployeeId && !loading && !error" class="user-info">
    <div class="image">
      <img src="../assets/placeholder.png" alt="Image" />
    </div>
    <div class="info">
      <h2 class="name">
        {{ user.name }}
      </h2>
      <div class="credentials">
        <div class="credentials__item">
          <span class="type">email:</span> <span class="value">{{ user.email }}</span>
        </div>
        <div class="credentials__item">
          <span class="type">phone:</span> <span class="value">{{ user.phone }}</span>
        </div>
      </div>
      <div class="about">
        <h3 class="about__title">
          О себе:
        </h3>
        <div class="about__text">
          <p>Hi 👋, my name is <span class="name">{{ user.name }}</span>, there is some useful information about me.</p>
          <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
        </div>
      </div>
    </div>
  </div>
  <div v-else class="empty">
    {{ message }}
  </div>
</template>

<style lang="scss">
.user-info {
  height: 100%;
  padding: 30px;
  padding-left: 20px;
  display: flex;
  gap: 60px;

  .image {
    width: 424px;
    height: 286px;
    flex-shrink: 0;

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }

  .info {
    .name {
      color: #000;
      font-size: 16px;
      font-style: normal;
      font-weight: 600;
      line-height: 140%; /* 22.4px */
      margin-bottom: 10px;
    }

    .credentials {
      display: flex;
      flex-direction: column;
      gap: 10px;
      margin-bottom: 20px;

      &__item {
        .type {
          color: #333;
          font-size: 14px;
          font-style: normal;
          font-weight: 600;
          line-height: 140%; /* 19.6px */
        }

        .value {
          color: #76787D;
          font-size: 14px;
          font-style: normal;
          font-weight: 400;
          line-height: 140%;
        }
      }
    }

    .about {
      &__title {
        color: #333;
        font-size: 16px;
        font-style: normal;
        font-weight: 600;
        line-height: 140%; /* 22.4px */
        margin-bottom: 25px;
      }

      &__text {
        display: flex;
        flex-direction: column;
        gap: 10px;
        color: #76787D;
        font-size: 14px;
        font-style: normal;
        font-weight: 400;
        line-height: normal;

        .name {
          font-size: 13px;
          font-weight: 600;
        }
      }
    }
  }
}

.empty {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #76787D;
  font-size: 14px;
  font-style: normal;
  font-weight: 400;
  line-height: normal;
}
</style>