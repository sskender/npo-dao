<template>
  <div class="wrap-token-holders">
    <h5>Active members</h5>
    <p id="chairman">
      Chairman: <br />
      <span class="account-address">{{ this.chairmanAddress }}</span>
    </p>
    <p id="holders">
      Total members: <span>{{ numberOfTokenHolders() }}</span>
    </p>
    <ul class="list-group list-group-flush">
      <li
        class="list-group-item"
        v-for="holder in tokenHolders()"
        :key="holder"
      >
        <span class="account-address"> {{ holder }} </span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "TokenHolders",
  data: () => {
    return { chairmanAddress: null };
  },
  methods: {
    async getChairman() {
      const contract = this.$store.state.DaoTokenContract;
      try {
        const chairman = await contract.methods.chairman().call();
        this.chairmanAddress = chairman;
      } catch (err) {
        console.error(err);
      }
    },
    numberOfTokenHolders() {
      return this.$store.getters.numberOfTokenHolders;
    },
    tokenHolders() {
      return this.$store.state.tokenHoldersList;
    },
  },
  async created() {
    await this.getChairman();
  },
};
</script>

<style scoped>
.wrap-token-holders {
  padding-top: 4rem;
}

p#chairman {
  padding-top: 0.5rem;
}

p#holders {
  padding-top: 0.5rem;
}
</style>

