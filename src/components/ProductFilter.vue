<template>
  <q-card>
    <q-card-section>
      <div class="row q-mt-md q-ml-md q-pr-md">
        <div class="col-md-4 col-sm-12 col-xs-12 q-pt-sm">
          <q-select
            :options="currencies"
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
            :options="categories.map((c) => c.category)"
            v-model="filterData.categories"
            filled
            multiple
            use-chips
            stack-label
            hint="Select Categories"
            label="Categories"
          ></q-select>
        </div>
      </div>
      <div class="row q-mt-md q-ml-md q-pr-md">
        <div class="col-12">
          <q-select
            v-model="filterData.merchants"
            filled
            hint="Select Merchant"
            label="Merchants"
          ></q-select>
        </div>
      </div>
    </q-card-section>
    <q-card-section>
      <div class="row q-mt-md q-ml-md q-pr-md">
        <div class="col-12">
          <q-checkbox
            v-model="filterData.freeShippingOnly"
            label="Free Shiping Only"
            size="sm"
            class="q-mt-sm"
          ></q-checkbox>
        </div>
      </div>
    </q-card-section>
    <q-separator />
    <q-card-actions align="right">
      <q-btn flat color="grey"> Clear </q-btn>
      <q-btn flat color="primary" class="q-mr-md"> Search </q-btn>
    </q-card-actions>
  </q-card>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "ProductFilter",
  props: ["filter", "currencies", "categories", "merchants", "stalls"],

  data: function () {
    return {
      filterData: {
        currency: null,
        priceFrom: 0,
        priceTo: null,
        categories: [],
        merchants: [],
        freeShippingOnly: false,
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
        freeShippingOnly: false,
      };
    },
  },
  created: async function () {
    console.log("### categories", this.categories);
    console.log("### currencies", this.currencies);

    this.filterData = {
      ...this.filterData,
      ...JSON.parse(JSON.stringify(this.filter || {})),
    };
    this.filterData.categories = (this.categories || [])
      .filter((c) => c.selected)
      .map((c) => c.category);
  },
});
</script>
