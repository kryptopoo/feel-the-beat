<template>
  <div>
    <div class="flex flex-col w-full">
      <div class="grid grid-cols-5 gap-4">
        <BeatCard v-for="(beatId, index) in beatIds" :key="index" :beatId="beatId" />
      </div>
    </div>
  </div>
</template>

<script>
import Utils from '@/js/utils'
import BeatCard from './BeatCard.vue'
import Beats from '@/js/beats'
export default {
  name: 'mybeats',
  components: { BeatCard },
  props: ['address'],
  data() {
    return {
      myAddress: this.$props['address'],
      beatIds: []
    }
  },
  computed: {},
  async created() {
    this.$root.$on('beatMinted', async () => {
      await this.loadBeats()
    })

    // console.log('MyBeats my address', this.myAddress)
    await Beats.init()
    await this.loadBeats()

    // Utils.getMyAddress(async (myAddress) => {
    //   self.myAddress = myAddress

    //   // get owner beatIds
    //   let tokensRs = await Beats.getTokensOfOwner(self.myAddress)
    //   tokensRs.forEach((tokenId) => {
    //     self.beatIds.push(tokenId.toNumber())
    //   })
    // })

    // Utils.getMyAddress((myAddress) => {
    //   self.myAddress = myAddress

    //   // get owner beatIds
    //   Beats.getTokensOfOwner(self.myAddress)
    //     .then((res) => {
    //       res.forEach((tokenId) => {
    //         self.beatIds.push(tokenId.toNumber())
    //       })
    //       console.log('beatIds', self.beatIds)
    //       // get token Uris
    //       self.beatIds.forEach((tokenId) => {
    //         Beats.get(tokenId)
    //           .then((tokenUri) => {
    //             console.log('tokenUri', tokenUri)
    //             self.tokenUris.push(tokenUri)
    //           })
    //           .catch((err) => {
    //             console.log(err)
    //           })
    //       })
    //     })
    //     .catch((err) => {
    //       console.log(err)
    //     })
    // })
  },
  methods: {
    loadBeats: function () {
      const self = this
      self.beatIds = []

      Utils.getMyAddress(async (myAddress) => {
        self.myAddress = myAddress

        // get owner beatIds
        let tokensRs = await Beats.getTokensOfOwner(self.myAddress)
        tokensRs.forEach((tokenId) => {
          self.beatIds.push(tokenId.toNumber())
        })
      })
    }
  }
}
</script>
