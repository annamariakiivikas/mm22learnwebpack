<template>
  <div class="row">
    <div class="col">
      <p class="text-center">Cookies {{ cookies }}</p>
      <img src="https://pngimg.com/d/cookie_PNG13656.png" class="img-fluid" @click="clickCookie">
    </div>
    <div class="col">
      Column
    </div>
    <div class="col">
      <button v-for="(upgrade, name) in upgrades"
        class="btn btn-outline-primary py-3"
        :disabled="cookies<upgrade.price"
        @click="buyUpgrade(upgrade)">
        Buy {{ name }} for 
        {{ upgrade.price }} clicks ({{upgrade.cps}} clicks per second)
        {{ upgrade.count }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  created() {
    setInterval(() => {
      for (const upgrade in this.upgrades) {
        this.cookies = (
          parseFloat(this.cookies) +
          this.upgrades[upgrade].cps * this.upgrades[upgrade].count
        ).toFixed(1);
      }
    }, 1000);
  },
  data() {
    return {
      cookies: 0,
      upgrades: {
        cursor: { price: 10, cps: 0.1, count: 0 },
        grandma: { price: 100, cps: 1, count: 0 },
        farm: { price: 1000, cps: 10, count: 0 },
        factory: { price: 10000, cps: 100, count: 0 },
        temple: { price: 100000, cps: 1000, count: 0 },
        clickDoubler: { price: 100, cps: 1*2, count: 0 }, // Click Doubler feature
      },
    };
  },
  methods: {
    clickCookie() {
      let clickValue = 1;
      if (this.upgrades.clickDoubler.count > 0) {
        clickValue *= 1* 2; // Double clicks if Click Doubler is active
      }
      this.cookies = (parseFloat(this.cookies) + clickValue).toFixed(1);
    },
    buyUpgrade(upgrade) {
      if (this.cookies >= upgrade.price) {
        this.cookies -= upgrade.price;
        upgrade.price += Math.ceil(upgrade.price * 0.25);
        upgrade.count++;
      }
    },
    buyClickDoubler() {
      const clickDoubler = this.upgrades.clickDoubler;
      if (this.cookies >= clickDoubler.price) {
        this.cookies -= clickDoubler.price;
        // Double the click value directly without relying on a count
        clickDoubler.cps = 2; 
      }
    },
  },
};
</script>
