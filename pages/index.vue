<template>
  <div justify="center">
    <div>
      <h3 class="primary--text">TABLE HEADING</h3>
    </div>
    <div>

    <div >
      <v-tabs  background-color = '#F2F0F9' color="accent">
        <v-tab class="text-capitalize accent--text">All</v-tab>
        <v-tab class="text-capitalize accent--text">Paid</v-tab>
        <v-tab class="text-capitalize accent--text">Unpaid</v-tab>
        <v-tab class="text-capitalize accent--text">Overdue</v-tab>
      <v-spacer></v-spacer>
      <span>

      Total payable amount: <span class="text-h6 primary--text">$900.00</span> USD
      </span>
      </v-tabs>
          <v-divider
            class=""
          ></v-divider>
          <div class="mt-5">
            <Table />
          </div>
    </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
}
</script>
