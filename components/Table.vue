<template>
  <v-data-table
    color="primary"
    :headers="dessertHeaders"
    v-model="selected"
    show-select
    :search="search"
    checkbox-color="primary"
    :single-select="singleSelect"
    :items="desserts"
    :single-expand="singleExpand"
    :expanded.sync="expanded"
    item-key="name"
    @item-expanded='itemExpanded'
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
        <v-text-field
          class="mt-5 ml-5"
          filled
          solo-inverted
          dense
          flat
        >
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
    <template v-slot:item.userstatus={item}>
      <div class="py-3">
         <v-chip
            class="primary--text "
            small
            color="#e6e6f2"
            
          >
            <v-icon color="" large>mdi-circle-small</v-icon>
            Active
          </v-chip><br>
        <span class="text-caption">Last login: 12/APR/2022</span> 
      </div>
      
    </template>
    <template v-slot:item.paystatus={item}>
      <div class="py-3">
         <v-chip
            class="green--text "
            small
            color="#cdffcd"
            
          >
            <v-icon color="" large>mdi-circle-small</v-icon>
            Paid
          </v-chip><br>
        <span class="text-caption">Paid On: 12/APR/2022</span> 
      </div>
      
    </template>
    <template v-slot:item.viewmore={item}>
      <div class="py-3">
        
        <span class="text-caption">View More</span> 
      </div>
      
    </template>
    <template v-slot:item.menu={item}>
       <v-icon color="">mdi-dots-vertical</v-icon>
      
      
    </template>
    <template v-slot:header.menu={header}>
       <v-icon color="">mdi-dots-vertical</v-icon>
      
      
    </template>

    <template v-slot:expanded-item="{ headers, item }">
      <td style="background-color:#F4F2FF" :colspan="headers.length" >
        <v-simple-table style="background-color:#F4F2FF">
    <template v-slot:default>
      <thead>
        <tr>
          <th class="text-left">
            DATE
          </th>
          <th class="text-left">
            USER ACTIVITY
          </th>
          <th class="text-left">
            DETAIL  <v-icon color="" small> mdi-information-outline</v-icon>
            
          </th>
         
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="item in desserts"
          :key="item.name"
        >
          <td>{{ item.name }}</td>
          <td>{{ item.calories }}</td>
        </tr>
      </tbody>
    </template>
  </v-simple-table></td>
      
    </template>
  </v-data-table>
</template>
<script>
export default {
  data() {
    return {
      expanded:[
      ],
      singleSelect: false,
      selected: [],
      search: '',
      singleExpand: true,
      sort: '',
      dessertHeaders: [
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
        { text: 'AMOUNT (g)',align:'center', value: 'carbs' },
        { text: '', value: 'viewmore' },
        { text: '', value: 'menu' },
      ],
      desserts: [
        {
          name: 'PAYMENT STSTUS',
          calories: 159,
          fat: 6.0,
          carbs: 24,
          protein: 4.0,
          iron: '1%',
        },
        {
          name: 'AMOUNT',
          calories: 237,
          fat: 9.0,
          carbs: 37,
          protein: 4.3,
          iron: '1%',
        },
        {
          name: '',
          calories: 262,
          fat: 16.0,
          carbs: 23,
          protein: 6.0,
          iron: 'view more',
        },
        {
          name: 'menu',
          calories: 305,
          fat: 3.7,
          carbs: 67,
          protein: 4.3,
          iron: '8%',
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
    itemExpanded(e){
      console.log(JSON.stringify(e))
    }
  }
}
</script>