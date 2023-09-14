
<script setup>
import UserInfo from '../components/UserInfo.vue';
import Sidebar from '../components/Sidebar.vue';
</script>

<template>
  <div class="container">
    <header class="header">
      <h1 class="title">Жилфонд</h1>
      <span class="user">Пользователь</span>
    </header>
    <div class="content">
      <Sidebar />
      <UserInfo />
    </div>
  </div>
</template>

<style lang="scss">
.container {
  padding: 40px 60px;

  .header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 30px;

    .title {
      color: #E31F24;
      font-size: 32px;
      font-style: normal;
      font-weight: 700;
      line-height: normal;
    }

    .user {
      color: #333;
      font-size: 16px;
      font-style: normal;
      font-weight: 400;
      line-height: normal;
    }
  }

  .content {
    height: 575px;
    border-radius: 10px;
    background: #FDFDFD;
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.10);
    display: flex;
  }
}
</style>