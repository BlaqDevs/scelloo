<template>
  <div justify="center">
    <div>
      <h3 class="primary--text">TABLE HEADING</h3>
    </div>
    <div>
      <div>
        <v-tabs background-color="#F2F0F9" color="accent">
          <v-tab class="text-capitalize accent--text" @click="users = allUsers"
            >All</v-tab
          >
          <v-tab class="text-capitalize accent--text" @click="users = paidUsers"
            >Paid</v-tab
          >
          <v-tab
            class="text-capitalize accent--text"
            @click="users = unpaidUsers"
            >Unpaid</v-tab
          >
          <v-tab
            class="text-capitalize accent--text"
            @click="users = dueUsers"
            >Overdue</v-tab
          >
          <v-spacer></v-spacer>
          <span>
            Total payable amount:
            <span class="text-h6 primary--text">${{totalAmount}}</span> USD
          </span>
        </v-tabs>
                <!-- {{selected[selected].id}} -->
        
        <v-divider class=""></v-divider>
        <div class="mt-5">
          <!-- <Table ref="table" /> -->
          <!-- {{users}} -->
          <v-data-table
            color="primary"
            :search="search"
            :headers="headers"
            v-model="selected"
            show-select
            checkbox-color="primary"
            :single-select="singleSelect"
            :items="users"
            :single-expand="singleExpand"
            :expanded.sync="expanded"
            item-key="id"
            :sort-by.sync="sort"
            show-expand
            class="elevation-1"
          >
            <template v-slot:top>
              <v-toolbar flat>
                <v-menu
                  offset-y
                  :close-on-content-click="false"
                >
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
                    <v-radio-group v-model="sort1" @change="sortBy" class="mt-n2">
                      <v-list-item class="mt-n5">
                        <label for="">Default</label>
                        <v-spacer></v-spacer><v-radio value=""></v-radio>
                      </v-list-item>
                      <v-list-item class="mt-n5">
                        <label for="">First Name</label><v-spacer></v-spacer
                        ><v-radio value="firstName"></v-radio>
                      </v-list-item>
                      <v-list-item class="mt-n5">
                        <label for="">Last Name</label><v-spacer></v-spacer
                        ><v-radio value="lastName"></v-radio>
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
                    <v-radio-group class="mt-n2" @change="sortBy($event)" v-model="sort2">
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

 <!-- <v-text-field
            label="Solo"
            solo
            dense
            class="search primary"
            prepend-inner-icon="mdi-magnify"
          ></v-text-field> -->
          <div width="392px">

                <v-text-field background-color="secondary" v-model="search" class="search "   solo dense flat>
                  <template #prepend-inner>
                    <v-icon class="" left>mdi-magnify</v-icon>
                  </template>
                  <template #label>
                    <span class="text-caption">
                      Search Users by Name, Email or Date
                    </span>
                  </template>
                </v-text-field>
          </div>
                <v-spacer></v-spacer>
                <v-btn @click='markPaid(selected[0].id)' class="font-weight-bold" color="primary">
                  Pay Dues
                </v-btn>
              </v-toolbar>
              <v-divider class=""></v-divider>
            </template>
            <template v-slot:item.firstName="{ item }">
              {{ item.firstName }} {{ item.lastName }} <br>
              <span class="text-caption font-weight-light">{{item.email}}</span>
            </template>
            <template v-slot:item.lastName="{ item }">
            </template>
            <template v-slot:item.email="{ item }">
            </template>
          
            <template v-slot:item.lastLogin="{ item }">
              <div class="py-3">
                <v-chip :class="item.userStatus" small :color="item.userStatus">
                  <v-icon large>mdi-circle-small</v-icon>
                  {{ item.userStatus }} </v-chip
                ><br />
                <span class="text-caption"
                  >Last login: {{ item.lastLogin }}</span
                >
              </div>
            </template>
            <template v-slot:item.amountInCents="{ item }">
              <div class="py-3">
                <span class="">${{ item.amountInCents/100 }}</span>
              </div>
            </template>
            <template v-slot:item.paystatus="{ item }">
              <div class="py-3">
                <v-chip
                  :class="item.paymentStatus"
                  small
                  :color="item.paymentStatus"
                >
                  <v-icon color="" large>mdi-circle-small</v-icon>
                  {{ item.paymentStatus }} </v-chip
                ><br />
                <span class="text-caption">Paid On: {{ item.paidOn }}</span>
              </div>
            </template>
            <template v-slot:item.viewmore="{ item }">
              <div class="py-3">
                <span class="text-caption">View More</span>
              </div>
            </template>
            <template v-slot:item.menu="{ item }">
              <v-menu
                offset-y
                :close-on-content-click="false"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-icon v-bind="attrs" v-on="on" color=""
                    >mdi-dots-vertical</v-icon
                  >
                </template>
                <v-list class="text-subtitle-1" height="129px" width="154px">
                  <v-list-item class="mt-n2"> Edit </v-list-item>
                  <v-list-item class="mt-n5"> View Profile </v-list-item>
                  <v-list-item class="mt-n5 success--text">
                    Activate User
                  </v-list-item>
                  <v-list-item class="mt-n5 red--text"> Delete </v-list-item>
                </v-list>
              </v-menu>
            </template>
            <template v-slot:header.menu="{ header }">
              <v-icon color="">mdi-dots-vertical</v-icon>
              <!-- {{users}} -->
            </template>

            <template v-slot:expanded-item="{ headers, item }">
              <td
                style="background-color: #f4f2ff"
                :colspan="headers.length"
                class="px-5"
              >
                <v-simple-table
                  v-if="item.activities != ''"
                  style="background-color: #f4f2ff"
                  class="mx-5"
                >
                  <template v-slot:default>
                    <thead>
                      <tr>
                        <th class="text-left">DATE</th>
                        <th class="text-left">USER ACTIVITY</th>
                        <th class="text-left">
                          DETAIL
                          <v-icon color="" small>
                            mdi-information-outline</v-icon
                          >
                        </th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="i in item.activities" :key="i">
                        <td>{{ i.date }}</td>
                        <td>{{ i.userActivity }}</td>
                        <td>{{ i.details }}</td>
                      </tr>
                    </tbody>
                  </template>
                </v-simple-table>
                <v-row
                  v-else
                  class="mx-auto my-5 accent--text"
                  justify="center"
                >
                  <div>
                    <h3>NO RECORDS FOUND</h3>
                  </div>
                </v-row>
              </td>
            </template>
          </v-data-table>
          
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      search:'',
      expanded: [],
      sort1:'default',
      sort2:'all',
      singleSelect: false,
      selected: [],
      users: [],
      totalAmount:0,
      allUsers: [],
      paidUsers: [],
      unpaidUsers: [],
      dueUsers: [],
      notPaid:[],
      singleExpand: true,
      sort: [],
      headers: [
        {
          text: '',
          align: 'start',
          value: 'data-table-expand',
        },
        {
          text: 'NAME',
          align: 'start',
          sortable: true,
          filterable: true,
          value: 'firstName',
        },
        { text: 'USER STATUS', align:'start', value: 'lastLogin',  },
        {
          text: '',
          align: 'start',
          sortable: true,
          // class:'d-none',
          filterable: true,
          value: 'email',
        },
        {
          text: '',
          align: 'start',
          // class:'d-none',
          sortable: true,
          value: 'lastName',
        },
        { text: 'PAYMENT STATUS',align:'start', value: 'paystatus',  },
        { text: 'AMOUNT', align: 'center', value: 'amountInCents',},
        { text: '', value: 'viewmore' },
        { text: '', value: 'menu' },
      ],
    }
  },
  methods: {
    searchTable (value, search, item) {
        return value != null &&
          search != null &&
          typeof value === 'string' && item != null
      },
    sortBy() {
      this.sort=[this.sort2, this.sort1 ]
 },
async markPaid(id){
await this.$axios.patch(`https://cornie-assessment.herokuapp.com/mark-paid/${id}`)

 }
  },
  computed:{
    filteredTable(){
      this.users.filter((user) =>{
        return user.firstName.toLowerCase().includes(this.search.toLowerCase())
      })
    }
  },
  async mounted() {
    const users = await this.$axios.get(
      'https://cornie-assessment.herokuapp.com/users/hMEDdQkPDuyISOz'
    )
    this.paidUsers = users.data.data.filter(
      (user) => user.paymentStatus === 'paid'
    )
    this.unpaidUsers = users.data.data.filter(
      (user) => user.paymentStatus === 'unpaid'
    )
    this.users = users.data.data
    this.allUsers = users.data.data
    this.dueUsers = users.data.data.filter(
      (user) => user.paymentStatus === 'overdue'
    )
    this.notPaid = users.data.data.filter((user)=> user.paymentStatus === 'overdue' || user.paymentStatus ===  'unpaid')
    
  this.notPaid.forEach(element => {
    this.totalAmount += element.amountInCents/100
   this.totalAmount = Math.round(this.totalAmount * 100) / 100
  });
    
  },
}
</script>
<style scoped>
.active {
  background-color: #e6e6f2;
  color: #6d5bd0;
}
.inactive {
  background: #fef0f9;
  color: #6e6893;
}
.paid {
  background: #cdffcd;
  color: #007f00;
}
.overdue {
  background: #fef0f9;
  color: #d30000;
}
.unpaid {
  background: #ffeccc;
  color: #ce8500;
}
.search{
  width: 392px;
  height: 40px;
  border-radius: 5px;
  margin-left: 20px ;
  /* background: url(); */
}
.search:hover{
  border: 1px solid;
  border-color: #6d5bd0
}

</style>
