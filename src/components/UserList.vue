<template>
  <!--  <img :src="gege" alt="" class="gege" />-->

  <a-list item-layout="horizontal" :data-source="props.userList">
    <template #renderItem="{ item }">
      <a-list-item>
        <a-card hoverable style="width: 240px">
          <template #cover>
            <img alt="example" :src="gege" />
          </template>
          <a-card-meta :title="item.userName">
            <template #description>{{ item.userProfile }}</template>
          </a-card-meta>
        </a-card>
      </a-list-item>
    </template>
  </a-list>
</template>

<script setup lang="ts">
import gege from "../assets/gege.webp";
import { withDefaults, defineProps } from "vue";

// defineProps not defined (no matter if "import defineProps is added")
// fixed: change Props.postList: any[] to never[]
interface Props {
  userList: never[];
}

const props = withDefaults(defineProps<Props>(), {
  userList: () => [],
});
</script>

<style scoped>
.gege {
  width: 200px;
}
</style>
