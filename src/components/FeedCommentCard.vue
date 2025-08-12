<script setup>
import ProfileImg from "./ProfileImg.vue";
import { useAuthenticationStore } from "@/stores/authentication";

const authenticationStore = useAuthenticationStore();

const props = defineProps({
  item: Object,
});
</script>

<template>
  <div class="cmtItemCont mb-3">
    <div class="cmtItemProfile d-flex gap-1">
      <profile-img
        :clsValue="'profile pointer'"
        :size="24"
        :pic="props.item.writerPic"
        :userId="props.item.writerUserId"
      />
      <div class="pointer fw-semibold">
        {{
          props.item.writerNickName
            ? props.item.writerNickName
            : props.item.writerUid
        }}
      </div>
    </div>
    <div class="cmtItemCtnt">
      <div class="d-flex justify-content-between">
        <div>{{ props.item.comment }}</div>
        <template
          v-if="
            authenticationStore.state.signedUser.userId ===
            props.item.writerUserId
          "
        >
          <i class="fa fa-trash pointer" @click="$emit('onDeleteComment')"></i>
        </template>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
