<script>
import PrimaryButton from "@/components/PrimaryButton";
import PrimaryToggleButton from "@/components/PrimaryToggleButton";

export default {
  name: "ChallengeTabHeader",
  components: {
    PrimaryButton,
    PrimaryToggleButton
  },
  data() {
    return {
      retryChallenge: false,
      isInChallenge: false,
      isShowAllVisible: false,
      isAutoECVisible: false,
      showAllChallenges: false,
      autoEC: false,
      compactEC: false,
    };
  },
  watch: {
    retryChallenge(newValue) {
      player.options.retryChallenge = newValue;
    },
    autoEC(newValue) {
      player.reality.autoEC = newValue;
    },
    showAllChallenges(newValue) {
      player.options.showAllChallenges = newValue;
    },
    compactEC(newValue) {
      player.options.compactEC = newValue;
    },
  },
  methods: {
    update() {
      this.retryChallenge = player.options.retryChallenge;
      this.showAllChallenges = player.options.showAllChallenges;
      this.isInChallenge = Player.isInAnyChallenge;
      this.isShowAllVisible = PlayerProgress.eternityUnlocked();
      this.isAutoECVisible = Perk.autocompleteEC1.canBeApplied;
      this.autoEC = player.reality.autoEC;
      this.compactEC = player.options.compactEC;
    },
    restartChallenge() {
      const current = Player.anyChallenge;
      if (Player.isInAnyChallenge) {
        current.exit(true);
        current.start();
      }
    },
    exitChallenge() {
      const current = Player.anyChallenge;
      if (Player.isInAnyChallenge) {
        current.exit(false);
      }
    },
  }
};
</script>

<template>
  <div class="l-challenges-tab__header">
    <div class="c-subtab-option-container">
      <PrimaryToggleButton
        v-model="retryChallenge"
        class="o-primary-btn--subtab-option"
        label="Automatically retry challenges:"
      />
      <PrimaryToggleButton
        v-model="compactEC"
        class="o-primary-btn--subtab-option"
        label="Compact Challenges:"
      />
      <PrimaryToggleButton
        v-if="isShowAllVisible"
        v-model="showAllChallenges"
        class="o-primary-btn--subtab-option"
        label="Show all known challenges:"
      />
      <PrimaryToggleButton
        v-if="isAutoECVisible"
        v-model="autoEC"
        class="o-primary-btn--subtab-option"
        label="Auto Eternity Challenges:"
      />
      <PrimaryButton
        v-if="isInChallenge"
        class="o-primary-btn--subtab-option"
        @click="restartChallenge"
      >
        Restart Challenge
      </PrimaryButton>
      <PrimaryButton
        v-if="isInChallenge"
        class="o-primary-btn--subtab-option"
        @click="exitChallenge"
      >
        Exit Challenge
      </PrimaryButton>
    </div>
  </div>
</template>

<style scoped>

</style>
