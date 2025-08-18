<script setup>
import { reactive, onMounted, onUnmounted } from "vue";
import { useFeedStore } from "@/stores/feed";
import { useAuthenticationStore } from "@/stores/authentication";
import FeedCard from "@/components/FeedCard.vue";
import { getFeedList } from "@/services/feedService";
import { bindEvent } from "@/utils/commonUtils";

const INFINITY_SCROLL_GAP = 500;

const feedStore = useFeedStore();

const state = reactive({
  isLoading: false,
  isFinish: false,
});

const data = {
  page: 1,
  rowPerPage: 20,
};

const handleScroll = () => {
  bindEvent(state, window, getData);
};

onMounted(() => {
  window.addEventListener("scroll", handleScroll);
  getData();
});

onUnmounted(() => {
  window.removeEventListener("scroll", handleScroll);
  feedStore.clearList();
});

const getData = async () => {
  state.isLoading = true;
  const params = {
    page: data.page++,
    row_per_page: data.rowPerPage,
  };
  const res = await getFeedList(params);
  if (res.status === 200) {
    const result = res.data.result;
    if (result && result.length > 0) {
      feedStore.addFeedList(result);
    }
    if (result.length < data.rowPerPage) {
      state.isFinish = true;
    }
  }
  state.isLoading = false;
};

// formData.append('pic', state.feed.pics[0])
// formData.append('pic', state.feed.pics[1])
// formData.append('pic', state.feed.pics[2])
</script>

<template>
  <section class="back_color">
    <div class="container d-flex flex-column align-items-center">
      <feed-card
        v-for="item in feedStore.feedList"
        :key="item.feedIdid"
        :item="item"
      ></feed-card>
      <div v-if="state.isLoading" class="loading display-none">
        <img :src="loadingImg" />
      </div>
    </div>
  </section>
</template>

<style scoped>
.back_color {
  background-color: #fafafa;
}
</style>
