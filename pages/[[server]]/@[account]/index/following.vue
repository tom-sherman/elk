<script setup lang="ts">
const { t } = useI18n()
const params = useRoute().params
const handle = $(computedEager(() => params.account as string))

definePageMeta({ name: 'account-following' })

const account = await fetchAccountByHandle(handle)
const paginator = account ? useMasto().v1.accounts.listFollowing(account.id, {}) : null

const isSelf = useSelfAccount(account)

if (account) {
  useHeadFixed({
    title: () => `${t('account.following')} | ${getDisplayName(account)} (@${account.acct})`,
  })
}
</script>

<template>
  <template v-if="paginator">
    <AccountPaginator :paginator="paginator" :relationship-context="isSelf ? 'following' : undefined" context="following" :account="account" />
  </template>
</template>
