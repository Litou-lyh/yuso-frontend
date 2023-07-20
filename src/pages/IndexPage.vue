<template>
  <div class="home">
    <a-input-search
      v-model:value="searchParams.text"
      placeholder="input search text"
      enter-button="Search"
      size="large"
      @search="onSearch"
    />
    <MyDivider />
    <a-tabs v-model:activeKey="activeKey" @change="onTabChange">
      <a-tab-pane key="post" tab="Posts">
        <PostList :post-list="postList" />
      </a-tab-pane>
      <a-tab-pane key="picture" tab="Pictures">
        <PictureList />
      </a-tab-pane>
      <a-tab-pane key="user" tab="Users">
        <UserList :user-list="userList" />
      </a-tab-pane>
    </a-tabs>
  </div>
</template>

<script setup lang="ts">
import { ref, watchEffect } from "vue";
import PostList from "@/components/PostList.vue";
import PictureList from "@/components/PictureList.vue";
import UserList from "@/components/UserList.vue";
import MyDivider from "@/components/MyDivider.vue";
import { useRoute, useRouter } from "vue-router";
import myAxios from "@/plugins/myAxios";

const postList = ref([]);

myAxios.post("/post/list/page/vo", {}).then((res) => {
  postList.value = res.records;
});

const userList = ref([]);

myAxios.post("/user/list/page/vo", {}).then((res) => {
  userList.value = res.records;
});

const router = useRouter();
const route = useRoute();
const initSearchParams = {
  text: "",
  pageSize: 10,
  pageNum: 1,
};
const searchParams = ref(initSearchParams);

watchEffect(() => {
  searchParams.value = {
    ...initSearchParams,
    text: route.params.category,
  } as never;
});

const onSearch = () => {
  router.push({
    query: searchParams.value,
  });
};

const activeKey = route.params.category;

const onTabChange = (key: string) => {
  router.push({
    path: `/${key}`,
    query: searchParams.value,
  });
};
</script>
