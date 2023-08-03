<template>
  <q-card>
    <q-card-section>
      <div class="row q-mt-md q-ml-md q-pr-md">
        <div class="col-md-4 col-sm-12 col-xs-12 q-pt-sm">
          <q-select
            :options="[...currencies].sort()"
            v-model="filterData.currency"
            filled
            hint="Select Currency"
            label="Currency"
          ></q-select>
        </div>
        <div class="col-md-4 col-sm-12 col-xs-12 q-pt-sm q-pl-md">
          <q-input
            v-model="filterData.priceFrom"
            type="number"
            label="Price From"
            hint="Price Starting At"
          ></q-input>
        </div>
        <div class="col-md-4 col-sm-12 col-xs-12 q-pt-sm q-pl-lg">
          <q-input
            v-model="filterData.priceTo"
            type="number"
            label="Price To"
            hint="Maximum Price"
          ></q-input>
        </div>
      </div>
    </q-card-section>
    <q-card-section>
      <div class="row q-mt-md q-ml-md q-pr-md">
        <div class="col-12">
          <q-select
            :options="[...categories.map((c) => c.category).sort()]"
            v-model="filterData.categories"
            filled
            multiple
            use-chips
            stack-label
            hint="Any of these Categories"
            label="Categories"
          ></q-select>
        </div>
      </div>
      <div class="row q-mt-md q-ml-md q-pr-md">
        <div class="col-12">
          <q-select
            :options="merchantProfiles"
            v-model="filterData.merchants"
            filled
            multiple
            use-chips
            stack-label
            hint="Any of these Merchant"
            label="Merchants"
          ></q-select>
        </div>
      </div>
    </q-card-section>
    <q-separator />
    <q-card-actions align="right">
      <q-btn @click="clear()" flat color="grey"> Clear </q-btn>
      <q-btn @click="search()" flat icon="search" color="primary" class="q-mr-md">
        Search
      </q-btn>
    </q-card-actions>
  </q-card>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "ProductFilter",
  props: [
    "filter",
    "currencies",
    "categories",
    "merchants",
    "profiles",
    "stalls",
  ],

  data: function () {
    return {
      merchantProfiles: [],
      filterData: {
        currency: null,
        priceFrom: 0,
        priceTo: null,
        categories: [],
        merchants: [],
      },
    };
  },
  watch: {
    categories(n, o) {
      this.filterData.categories = (n || [])
        .filter((c) => c.selected)
        .map((c) => c.category);
    },
  },
  methods: {
    clear: function () {
      this.filterData = {
        currency: null,
        priceFrom: 0,
        priceTo: null,
        categories: [],
        merchants: [],
      };
      this.$emit("filter", this.filterData);
    },
    search: function () {
      this.$emit("filter", this.filterData);
    },
  },
  created: async function () {
    this.filterData = {
      ...this.filterData,
      ...JSON.parse(JSON.stringify(this.filter || {})),
    };
    this.filterData.categories = (this.categories || [])
      .filter((c) => c.selected)
      .map((c) => c.category);

    this.merchantProfiles = this.merchants.map((m) => {
      const merchantProfile = this.profiles.find((p) => p.pubkey === m);
      if (merchantProfile) {
        return {
          label:
            merchantProfile.name +
            ` (${m.substring(0, 10)}...${m.substring(m.length - 10)})`,
          value: m,
        };
      }
      return { label: m, value: m };
    });
  },
});
</script>
