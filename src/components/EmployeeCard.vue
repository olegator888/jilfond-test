<script setup>
import { computed } from "vue";

const props = defineProps(["user", "onSelect", "isSelected"]);

console.log(props.isSelected);

const cardContainerClassName = computed(() => `cardContainer ${props.isSelected && "selected"}`);
const cardInfoClassName = computed(() => `card__info ${props.isSelected && "selected"}`);
</script>

<template>
  <div :class="cardContainerClassName" @click="onSelect">
    <div class="card">
      <div class="card__image">
        <img src="../assets/placeholder.png" alt="Image">
      </div>
      <div :class="cardInfoClassName">
        <h4 class="name">{{ props.user.name }}</h4>
        <p class="email">{{ props.user.email }}</p>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
.card {
  background: #FFF;
  display: flex;
  position: relative;
  box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.10);
  border-radius: 10px;

  &Container {
    border: 1px solid transparent;
    border-radius: 10px;
    transition: border 200ms ease;
    cursor: pointer;

    &.selected,
    &:hover {
      border-color: #E0E0E0;
    }
  }

  &__image {
    height: 70px;
    width: 70px;
    flex-shrink: 0;
    border-radius: 10px 0 0 10px;
    overflow: hidden;

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }

  &__info {
    width: 100%;
    border-left: 1px solid #E0E0E0;
    padding: 15px 15px 20px 15px;
    border-radius: 0 9px 9px 0;
    transition: background-color 200ms ease;

    &.selected {
      background: #E0E0E0;
    }

    .name,
    .email {
      max-width: 130px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .name {
      color: #333;
      font-size: 14px;
      font-style: normal;
      font-weight: 600;
      line-height: normal;
    }

    .email {
      color: #76787D;
      font-size: 14px;
      font-style: normal;
      font-weight: 400;
      line-height: normal;
    }
  }
}
</style>