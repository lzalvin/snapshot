<script setup lang="ts">
import { defineEmits } from 'vue';
import { useDelegate } from '@/composables/useDelegate';
import { sleep } from '@snapshot-labs/snapshot.js/src/utils';

const props = defineProps<{
  open: boolean;
  userAddress: string;
  spaceId: string;
}>();

const emit = defineEmits(['close', 'reload']);

const { delegateTo, delegationLoading } = useDelegate();

async function handleDelegate() {
  emit('close');
  await delegateTo(props.userAddress, props.spaceId);
  await sleep(5000);
  emit('reload');
}
</script>

<template>
  <BaseModal :open="open" @close="$emit('close')">
    <template v-slot:header>
      <div class="flex flex-row justify-center items-center">
        <h3>{{ $t('profile.about.delegate') }}</h3>
      </div>
    </template>
    <div class="p-4 space-y-3">
      <SBaseInput
        :modelValue="userAddress"
        :title="$t('profile.about.delegateTo')"
        readonly
      >
        <template v-slot:label>{{ $t('delegate.to') }}</template>
      </SBaseInput>
      <SBaseInput :modelValue="spaceId" :title="$t('space')" readonly>
        <template v-slot:label>{{ $t('space') }}</template>
      </SBaseInput>
    </div>
    <div class="p-4">
      <BaseButton
        primary
        @click="handleDelegate"
        :loading="delegationLoading"
        class="w-full"
      >
        {{ $t('confirm') }}
      </BaseButton>
    </div>
  </BaseModal>
</template>
