<script setup lang="ts">
import { toHex } from 'web3-utils'
import UP, { UPAuthMessage, UPAuthResponse } from 'up-core-test'
import UPCKB from 'up-ckb-alpha-test'
// import PWCore, {
//   Address,
//   AddressType,
//   Amount,
//   ChainID,
//   IndexerCollector,
// } from '@lay2/pw-core'
import { useUserStore } from '~/stores/user'

onMounted(() => {
  UP.config({
    domain: 'app.unipass.id',
  })
  // PWCore.setChainId(0)
  // UPCKB.config({
  //   upSnapshotUrl: 'https://d.aggregator.unipass.id/dev/snapshot/',
  //   chainID: 1,
  //   ckbIndexerUrl: 'https://testnet.ckb.dev/indexer',
  //   ckbNodeUrl: 'https://testnet.ckb.dev',
  //   upLockCodeHash:
  //         '0xd41445a4845a09c163d174f59644877465710031582f640ba2e11437b005b812',
  // })
})

const user = useUserStore()
const name = ref(user.savedName)

const router = useRouter()
const go = async() => {
  console.log('🌊', toHex(name.value))
  UP.disconnect()
  const account = await UP.connect({ email: true, evmKeys: true })
  console.log('🌊', account)
  // console.log('🌊', PWCore)
}

const { t } = useI18n()
</script>

<template>
  <div>
    <div text-4xl>
      <div i-carbon-campsite inline-block />
    </div>
    <p>
      <a rel="noreferrer" href="https://github.com/antfu/vitesse" target="_blank">Vitesse</a>
    </p>
    <p>
      <em text-sm opacity-75>{{ t("intro.desc") }}</em>
    </p>

    <div py-4 />

    <input
      id="input"
      v-model="name"
      :placeholder="t('intro.whats-your-name')"
      :aria-label="t('intro.whats-your-name')"
      type="text"
      autocomplete="false"
      p="x4 y2"
      w="250px"
      text="center"
      bg="transparent"
      border="~ rounded gray-200 dark:gray-700"
      outline="none active:none"
      @keydown.enter="go"
    >
    <label class="hidden" for="input">{{ t("intro.whats-your-name") }}</label>

    <div>
      <button btn m-3 text-sm :disabled="!name" @click="go">
        {{ t("button.go") }}
      </button>
    </div>
  </div>
</template>

<route lang="yaml">
meta:
  layout: home
</route>
