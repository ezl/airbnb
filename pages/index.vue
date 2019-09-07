<template>

  <div class="homePage">
    <div class="exploreBox">

      <!-- Header -->
      <h1 class="boxHeader">Find places to stay on Airbnb</h1>

      <div class="headerDescription">Discover entire homes and private rooms perfect for any trip.</div>

      <!-- Form -->
      <v-form class="detailsForm" ref="form" v-model="valid">
        <!-- Where -->
        <v-text-field label="Where" outlined></v-text-field>

        <!-- Checkin Checkout -->
        <v-row>
          <!-- Checkin -->
          <v-col>
            <v-menu v-model="checkinMenu" :close-on-content-click="false" transition="scale-transition" offset-y
              full-width max-width="290px" min-width="290px">
              <template v-slot:activator="{ on }">
                <v-text-field v-model="checkoinDate" label="CHECK-IN" prepend-inner-icon="event" outlined readonly
                  v-on="on"></v-text-field>
              </template>
              <v-date-picker v-model="checkoinDate" no-title @input="checkinMenu = false"></v-date-picker>
            </v-menu>
            </v-layout>
          </v-col>
          <!-- Checkout -->
          <v-col>
            <v-menu v-model="checkoutMenu" :close-on-content-click="false" transition="scale-transition" offset-y
              full-width max-width="290px" min-width="290px">
              <template v-slot:activator="{ on }">
                <v-text-field v-model="checkoutDate" label="CHECK-OUT" prepend-inner-icon="event" outlined readonly
                  v-on="on"></v-text-field>
              </template>
              <v-date-picker v-model="checkoutDate" no-title @input="checkoutMenu = false"></v-date-picker>
            </v-menu>
            </v-layout>
          </v-col>
        </v-row>

        <!-- Adults/Childrens -->
        <v-row>
          <!-- Adults -->
          <v-col>
            <v-select v-model="adults" :items="adultsList" item-text="label" item-value="value" label="Adults" outlined>
            </v-select>
          </v-col>
          <!-- Childrens -->
          <v-col>
            <v-select v-model="childs" :items="childsList" item-text="label" item-value="value" label="Childs" outlined>
            </v-select>
          </v-col>
        </v-row>

        <!-- Search Button -->
        <div class="searchBtnContainer">
          <v-btn prepend-inner-icon="magnify" depressed x-large large>
            <v-icon left>mdi-magnify</v-icon>
            Search
          </v-btn>
        </div>

      </v-form>


    </div>
  </div>

</template>

<script>
  export default {
    data: () => ({

      adults: null,
      adultsList: [{ value: 1, label: "1 adult" },
      { value: 2, label: "2 adults" },
      { value: 3, label: "3 adults" },
      { value: 4, label: "4 adults" },
      { value: 5, label: "5 adults" },
      { value: 6, label: "6 adults" },
      { value: 7, label: "7 adults" },
      { value: 8, label: "8 adults" },
      { value: 9, label: "9 adults" },
      { value: 10, label: "10 adults" },
      { value: 11, label: "11 adults" },
      { value: 12, label: "12 adults" },
      { value: 13, label: "13 adults" },
      { value: 14, label: "14 adults" },
      { value: 15, label: "15 adults" },
      { value: 16, label: "16 adults" }],
      childs: null,
      childsList: [{ value: 1, label: "1 child" },
      { value: 2, label: "2 childs" },
      { value: 3, label: "3 childs" },
      { value: 4, label: "4 childs" },
      { value: 5, label: "5 childs" },
      ],

      checkoinDate: null,
      checkinMenu: false,

      checkoutDate: null,
      checkoutMenu: false
    })
  }
</script>
<style lang="scss">
  body {
    height: 100vh;
    overflow: hidden;
    font-family: roboto;

    .homePage {
      background-image: url("https://images.unsplash.com/photo-1505691938895-1758d7feb511?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80");
      background-repeat: no-repeat;
      background-size: cover;
      background-position: center center;
      min-height: 100vh;
      overflow: hidden;

      display: flex;
      align-items: center;
      /* justify-content: center; */


      .exploreBox {
        margin-left: 10%;
        width: 500px;
        padding: 25px;

        background: white;
        border-radius: 3px;

        .boxHeader {
          color: #555;
          font-weight: 400;
        }

        .headerDescription {
          font-weight: 300;
          margin: 5px 0px 10px 0px;
          ;
        }

        .detailsForm {
          margin-top: 25px;

          .v-text-field__details {
            display: none;
          }

          .searchBtnContainer {
            text-align: right;

            button {
              background: #009688 !important;
              color: white !important;
            }
          }
        }

      }
    }
  }
</style>