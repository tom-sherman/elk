<script setup lang="ts">
import type { UserLogin } from '~/types'

const all = useUsers()

const router = useRouter()
const masto = useMasto()
const switchUser = (user: UserLogin) => {
  if (user.account.id === currentUser.value?.account.id)
    router.push(getAccountRoute(user.account))
  else
    masto.loginTo(user)
}
</script>

<template>
  <div flex justify-start items-end px-2 gap-5>
    <div flex="~ wrap-reverse" gap-5>
      <template v-for="user of all" :key="user.id">
        <CommonTooltip :distance="8" :delay="{ show: 300, hide: 100 }">
          <button
            flex rounded
            cursor-pointer
            aria-label="Switch user"
            :class="user.account.id === currentUser?.account.id ? '' : 'op25 grayscale'"
            hover="filter-none op100"
            @click="switchUser(user)"
          >
            <AccountAvatar w-13 h-13 :account="user.account" square />
          </button>

          <template #popper>
            <div text-center>
              <span text-4>
                <AccountDisplayName :account="user.account" />
              </span>
              <AccountHandle :account="user.account" />
            </div>
          </template>
        </CommonTooltip>
      </template>
    </div>
    <div flex items-center justify-center w-13 h-13>
      <UserDropdown />
    </div>
  </div>
</template>
