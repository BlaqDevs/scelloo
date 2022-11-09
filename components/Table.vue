<template>
  <v-data-table
    color="primary"
    :headers="headers"
    v-model="selected"
    show-select
    :search="search"
    checkbox-color="primary"
    :single-select="singleSelect"
    :items="users"
    single-expand=true
    :expanded.sync="expanded"
    item-key="firstName"
    show-expand
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-menu :rounded="rounded" offset-y :close-on-content-click="false">
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              class="text-capitalize"
              outlined
              color="accent"
              v-bind="attrs"
              v-on="on"
            >
              <v-icon left color="">mdi-filter</v-icon>
              Filter
            </v-btn>
          </template>
          <v-list class="text-subtitle-1" width="224px">
            <v-list-item>
              <v-list-item-title class="accent--text font-weight-light">
                SORT BY:
              </v-list-item-title>
            </v-list-item>
            <v-radio-group v-model="sort1" class="mt-n2">
              <v-list-item class="mt-n5">
                <label for="">Default</label>
                <v-spacer></v-spacer><v-radio value="default"></v-radio>
              </v-list-item>
              <v-list-item class="mt-n5">
                <label for="">First Name</label><v-spacer></v-spacer
                ><v-radio value="first-name"></v-radio>
              </v-list-item>
              <v-list-item class="mt-n5">
                <label for="">Last Name</label><v-spacer></v-spacer
                ><v-radio value="last-name"></v-radio>
              </v-list-item>
              <v-list-item class="mt-n5">
                <label for="">Due Date</label><v-spacer></v-spacer
                ><v-radio value="due-date"> </v-radio>
              </v-list-item>
              <v-list-item class="mt-n5">
                <label for="">Last Login</label><v-spacer></v-spacer
                ><v-radio value="last-login"></v-radio>
              </v-list-item>
            </v-radio-group>
            <v-divider class="mt-n5"></v-divider>
            <v-list-item class="accent--text font-weight-light"
              >USERS:</v-list-item
            >
            <v-radio-group class="mt-n2" v-model="sort2">
              <v-list-item class="mt-n5">
                <label for="">All</label><v-spacer></v-spacer
                ><v-radio value="all"></v-radio>
              </v-list-item>
              <v-list-item class="mt-n5">
                <label for="">Active</label><v-spacer></v-spacer
                ><v-radio value="active"></v-radio>
              </v-list-item>
              <v-list-item class="mt-n5">
                <label for="">Inactive</label><v-spacer></v-spacer
                ><v-radio value="inactive"></v-radio>
              </v-list-item>
            </v-radio-group>
          </v-list>
        </v-menu>
        <v-text-field class="mt-5 ml-5" filled solo-inverted dense flat>
          <template #prepend-inner>
            <v-icon color="" left>mdi-magnify</v-icon>
          </template>
          <template #label>
            <p class="text-caption">Search Users by Name, Email or Date</p>
          </template>
        </v-text-field>
        <v-spacer></v-spacer>

        <v-btn class="font-weight-bold" color="primary"> Pay Dues </v-btn>
      </v-toolbar>
      <v-divider class=""></v-divider>
    </template>
    <template v-slot:item.name="{ item }">
      {{item.firstName}} {{item.lastName}}
    </template>
    <template v-slot:item.userstatus="{ item }">
      <div class="py-3">
        <v-chip :class="item.userStatus" small :color="item.userStatus">
          <v-icon  large>mdi-circle-small</v-icon>
          {{item.userStatus}} </v-chip
        ><br />
        <span class="text-caption">Last login: {{item.lastLogin}}</span>
      </div>
    </template>
    <template v-slot:item.paystatus="{ item }">
      <div class="py-3">
        <v-chip :class="item.paymentStatus" small :color="item.paymentStatus">
          <v-icon color="" large>mdi-circle-small</v-icon>
          {{item.paymentStatus}} </v-chip
        ><br />
        <span class="text-caption">Paid On: {{item.paidOn}}</span>
      </div>
    </template>
    <template v-slot:item.viewmore="{ item }">
      <div class="py-3">
        <span class="text-caption">View More</span>
      </div>
    </template>
    <template v-slot:item.menu="{ item }">
      <v-menu :rounded="rounded" offset-y :close-on-content-click="false">
        <template v-slot:activator="{ on, attrs }">
          <v-icon v-bind="attrs" v-on="on" color="">mdi-dots-vertical</v-icon>
        </template>
        <v-list class="text-subtitle-1" height="129px" width="154px">
          <v-list-item class="mt-n2"> Edit </v-list-item>
          <v-list-item class="mt-n5"> View Profile </v-list-item>
          <v-list-item class="mt-n5 success--text"> Activate User </v-list-item>
          <v-list-item class="mt-n5 red--text"> Delete </v-list-item>
        </v-list>
      </v-menu>
    </template>
    <template v-slot:header.menu="{ header }">
      <v-icon color="">mdi-dots-vertical</v-icon>
      <!-- {{users}} -->
    </template>

    <template v-slot:expanded-item="{ headers, item }">
      <td  style="background-color: #f4f2ff" :colspan="headers.length" class="px-5">
        <v-simple-table v-if="item.activities != null" style="background-color: #f4f2ff" class="mx-5">
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">DATE</th>
                <th class="text-left">USER ACTIVITY</th>
                <th class="text-left">
                  DETAIL
                  <v-icon color="" small> mdi-information-outline</v-icon>
                </th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="i in item.activities" :key="i">
                <td>{{ i.date }}</td>
                <td>{{i.userActivity }}</td>
                <td>{{i.details }}</td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
        <v-row v-else class="mx-auto my-5 accent--text" justify='center'>
 <div >
        <h3 >NO RECORDS FOUND</h3>
        </div>
        </v-row>
       
      </td>
    </template>
  </v-data-table>
</template>
<script>
export default {
  data() {
    return {
      expanded:[
      ],
      sortUser:'all',
      singleSelect: false,
      selected: [],
      users:[],
      paidUsers:[],
      unpaidUsers:[],
      dueausers:[],
      search: '',
      singleExpand: true,
      sort: '',
      headers: [
        {
          text: '',
          align: 'start',
          value: 'data-table-expand',
        },
        {
          text: 'NAME',
          align: 'start',
          sortable: false,
          value: 'name',
        },
        { text: 'USER STATUS', value: 'userstatus' },
        { text: 'PAYMENT STATUS', value: 'paystatus' },
        { text: 'AMOUNT (g)',align:'center', value: 'amountInCents' },
        { text: '', value: 'viewmore' },
        { text: '', value: 'menu' },
      ],
      desserts: [
          {
            name: 'Frozen Yogurt',
            calories: 159,
            fat: 6.0,
            carbs: 24,
            protein: 4.0,
            iron: '1%',
          },
          {
            name: 'Ice cream sandwich',
            calories: 237,
            fat: 9.0,
            carbs: 37,
            protein: 4.3,
            iron: '1%',
          },
          {
            name: 'Eclair',
            calories: 262,
            fat: 16.0,
            carbs: 23,
            protein: 6.0,
            iron: '7%',
          },
          {
            name: 'Cupcake',
            calories: 305,
            fat: 3.7,
            carbs: 67,
            protein: 4.3,
            iron: '8%',
          },
          {
            name: 'Gingerbread',
            calories: 356,
            fat: 16.0,
            carbs: 49,
            protein: 3.9,
            iron: '16%',
          },
          {
            name: 'Jelly bean',
            calories: 375,
            fat: 0.0,
            carbs: 94,
            protein: 0.0,
            iron: '0%',
          },
          {
            name: 'Lollipop',
            calories: 392,
            fat: 0.2,
            carbs: 98,
            protein: 0,
            iron: '2%',
          },
          {
            name: 'Honeycomb',
            calories: 408,
            fat: 3.2,
            carbs: 87,
            protein: 6.5,
            iron: '45%',
          },
          {
            name: 'Donut',
            calories: 452,
            fat: 25.0,
            carbs: 51,
            protein: 4.9,
            iron: '22%',
          },
          {
            name: 'KitKat',
            calories: 518,
            fat: 26.0,
            carbs: 65,
            protein: 7,
            iron: '6%',
          },
        ],
    }
  },
  methods:{
  sortUser(){
    alert('data')
  // this.users = data
}
  },
    async mounted(){
    const users= await this.$axios.get("https://cornie-assessment.herokuapp.com/users/hMEDdQkPDuyISOz")
this.paidUsers = users.data.data.filter((user)=> user.paymentStatus === 'paid')
this.unpaidUsers = users.data.data.filter((user)=> user.paymentStatus === 'unpaid')
this.users = users.data.data
this.dueUsers = users.data.data.filter((user)=> user.paymentStatus === 'overdue')
},
}
</script>
<style scoped>
.active{
  background-color: #e6e6f2;
  color: #6D5BD0
}
.inactive{
  background: #fef0f9;
  color: #6e6893
}
.paid{
  background: #CDFFCD;
  color: #007F00
}
.overdue{
  background: #fef0f9;
  color: #D30000
}
.unpaid{
  background: #FFECCC;
  color: #CE8500
}

</style>