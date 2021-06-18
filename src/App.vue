<template>
  <v-app>
    <v-app-bar app lighten>
      <div class="d-flex align-center pl-10 pa-2">
        <h2 class="light-blue--text text--darken-4 text-company">Shiba Book Shop</h2>
      </div>
      <v-spacer></v-spacer>
      <div class="pa-2" v-if="count == '0'">
        <v-icon color="light-blue darken-4">shopping_cart
        </v-icon>
      </div>
      <div class="pa-2" v-else>
        <v-badge color="error" :content="this.count" tile overlap>
          <v-icon color="light-blue darken-4">shopping_cart
          </v-icon>
        </v-badge>
      </div>
      <div class="pa-2 pr-10 cursor" @click="drawer = !drawer">
        <strong class="light-blue--text text--darken-4 text-company">฿ {{total()-Discount()}}</strong>
      </div>
    </v-app-bar>
    <v-navigation-drawer app v-model="drawer" right width="500px">
      <!-- <template v-slot:prepend> -->
      <v-list-item>
        <v-icon @click="drawer = !drawer" class="hidden-md-and-up">arrow_back</v-icon>
        <h2 class="light-blue--text text--darken-4 pl-3">Cart</h2>

      </v-list-item>
      <!-- </template> -->
      <v-container>
        <div class="p-2">
          <v-row>
            <v-col xs='4' sm="4" md="4" lg="4">
              <h4 class="text-center">Title</h4>
            </v-col>
            <v-col xs='2' sm="2" md="2" lg="2">
              <h4 class="text-center">Price</h4>
            </v-col>
            <v-col xs='3' sm="3" md="3" lg="3">
              <h4 class="text-center">Amount</h4>
            </v-col>
            <v-col xs='2' sm="2" md="2" lg="2">
              <h4 class="text-center">Total</h4>
            </v-col>
            <v-col xs='1' sm="1" md="1" lg="1">

            </v-col>
          </v-row>
          <v-list v-for="item in cards">
            <v-row>
              <v-col xs='4' sm="4" md="4" lg="4">
                <p>{{ item.title }}</p>
              </v-col>
              <v-col xs='2' sm="2" md="2" lg="2">
                <p class="text-center">{{ item.price }}</p>
              </v-col>
              <v-col xs='3' sm="3" md="3" lg="3">
                <div class="text-center">
                  <v-icon @click="removeqty(item)" class="icon">remove</v-icon>
                  {{ item.qty }}
                  <v-icon @click="addqty(item)" class="icon">add</v-icon>
                </div>
              </v-col>

              <v-col xs='2' sm="2" md="2" lg="2">
                <p class="text-center">{{ item.total }}</p>
              </v-col>
              <v-col xs='1' sm="1" md="1" lg="1">
                <v-icon @click="remove(item)">delete</v-icon>
              </v-col>
            </v-row>
          </v-list>
          <v-row>
            <v-col xs='6' sm="6" md="6" lg="6">
              <h4 class="text-right">Discount :</h4>
            </v-col>
            <v-col xs='6' sm="6" md="6" lg="6">
              <h4>฿ {{Discount()}}</h4>
            </v-col>
          </v-row>
          <v-row>
            <v-col xs='6' sm="6" md="6" lg="6">
              <h4 class="text-right">Net :</h4>
            </v-col>
            <v-col xs='6' sm="6" md="6" lg="6">
              <h4>฿ {{total()-Discount()}}</h4>
            </v-col>

          </v-row>
          <v-btn outlined color="red accent-4" block @click="Payment()" v-if="total() > 0">Payment</v-btn>
        </div>
      </v-container>
    </v-navigation-drawer>
    <v-main class="background-body">
      <v-container>
        <v-row v-for="item in data">
          <v-col xs='6' sm="6" md="3" lg="3" v-for="items in item">
            <v-hover v-slot="{ hover }" close-delay="">
              <v-card class="mx-auto pa-3" :elevation="hover ? 16 : 2">
                <v-img :src="items.cover" class="img"></v-img>
                <v-card-text>
                  <h4 v-text="items.title"></h4>
                </v-card-text>
                <v-card-text>
                  <h3 v-text="'฿ '+ items.price + '.00'"></h3>
                </v-card-text>

                <v-card-action>

                  <v-btn block color="warning" @click="addCard(items)">
                    <v-icon>shopping_cart</v-icon>add to cart
                  </v-btn>
                </v-card-action>
              </v-card>
            </v-hover>
          </v-col>
        </v-row>
      </v-container>


      <v-dialog v-model="dialog" persistent max-width="400">
        <v-card>
          <v-card-title class="text-h5 light-green--text text--darken-1">
            Successful
          </v-card-title>
          <v-card-text>
            <v-container>
              <div class="p-2">
                <v-row>
                  <v-col xs='4' sm="4" md="4" lg="4">
                    <h4 class="text-center">Title</h4>
                  </v-col>
                  <v-col xs='3' sm="3" md="3" lg="3">
                    <h4 class="text-center">Price</h4>
                  </v-col>
                  <v-col xs='3' sm="3" md="3" lg="3">
                    <h4 class="text-center">Amount</h4>
                  </v-col>
                  <v-col xs='2' sm="2" md="2" lg="2">
                    <h4 class="text-center">Total</h4>
                  </v-col>
                </v-row>
                <v-list v-for="item in cards">
                  <v-row>
                    <v-col xs='4' sm="4" md="4" lg="4">
                      <p class="text-center">{{ item.title }}</p>
                    </v-col>
                    <v-col xs='3' sm="3" md="3" lg="3">
                      <p class="text-center">{{ item.price }}</p>
                    </v-col>
                    <v-col xs='3' sm="3" md="3" lg="3">
                     
                      <p class="text-center">{{ item.qty }}</p>
                        
                        
                      
                    </v-col>

                    <v-col xs='2' sm="2" md="2" lg="2">
                      <p class="text-center">{{ item.total }}</p>
                    </v-col>
                  
                  </v-row>
                </v-list>
                <v-row>
                  <v-col xs='6' sm="6" md="6" lg="6">
                    <h4 class="text-right">Discount :</h4>
                  </v-col>
                  <v-col xs='6' sm="6" md="6" lg="6">
                    <h4>฿ {{Discount()}}</h4>
                  </v-col>
                </v-row>
                <v-row>
                  <v-col xs='6' sm="6" md="6" lg="6">
                    <h4 class="text-right">Net :</h4>
                  </v-col>
                  <v-col xs='6' sm="6" md="6" lg="6">
                    <h4>฿ {{total()-Discount()}}</h4>
                  </v-col>
                </v-row>

              </div>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="green darken-1" text @click="close()">
              OK
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>


    </v-main>
  </v-app>
</template>

<script>
  export default {
    name: 'App',

    components: {

    },

    data: () => ({
      drawer: false,
      data: {},
      cards: [],
      count: 0,
      count1: 0,
      snackbar: false,
      timeout: 2000,
      dialog: false,

      //
    }),
    mounted() {
      this.getdata()
    },
    methods: {
      close () {
        this.dialog = false
        this.cards= []
        this.count = 0
      },
      Payment() {
        Swal.fire(
          'Payment',
          'You clicked the button!',
          'success'
        ).then(response =>{
          this.dialog = true
          
        })
        
      },
      getdata() {
        this.axios.get('https://akita-examination.s3-ap-southeast-1.amazonaws.com/shiba-book-shop.json', {
        }).then((response) => {
          if (response.status == 200) {
            this.data = response.data
          }
        })
      },
      addCard: function (item) {
        var id = item.id
        let existID = this.cards.map(i => { return i.id });
        if (existID.includes(item.id)) {
          var found = this.findIndex(item)
          this.cards[found].qty += 1;
          this.cards[found].total = this.cards[found].qty * this.cards[found].price;
          this.snackbar = true

        }
        else {
          this.pushData(item)
          this.count += 1
          this.snackbar = true
        }
        this.Discount()
      },
      pushData(product) {
        this.cards.push({
          id: product.id,
          cover: product.cover,
          title: product.title,
          price: product.price,
          qty: 1,
          total: product.price
        })
      },
      findIndex: function (item) {
        for (var i = 0; i < this.cards.length; i++) {
          if (this.cards[i].id == item.id) {
            return i;
          }
        }
        return -1;
      },
      removeqty: function (items) {
        items.qty -= 1;
        if (items.qty <= 1) {
          items.qty = 1;
        }
        items.total = items.price * items.qty
      },
      addqty: function (items) {
        items.qty += 1;
        items.total = items.price * items.qty
      },
      remove(items) {
        var index = this.cards.indexOf(items);
        this.cards.splice(index, 1)
        this.count = 0
      },
      total: function () {
        var sum = 0;
        this.cards.forEach(function (item) {
          sum = parseInt(parseInt(sum) + parseInt(item.total));
        });
        return sum
      },
      Discount: function () {
        let allDiscount = 0;
        let countUniq = 0;

        let allHarry = this.cards.filter(i => {
          if (i.id != '9780241392362') {
            return i;
          }
        });
        if (allHarry.length == 2) {
          let price = 0;
          allHarry.forEach(i => {
            price = parseInt(parseInt(price) + parseInt(i.price));
            countUniq = countUniq + i.qty;
          });
          let discount = Math.floor(countUniq / 2); // give discount 5%2 = 1 , 5/2 = 2
          allDiscount = price * discount * 0.1; // discount 10 %
        }
        if (allHarry.length == 3) {
          let price = 0;
          allHarry.forEach(i => {
            price = parseInt(parseInt(price) + parseInt(i.price));
            countUniq = countUniq + i.qty;
          });
          let discount = Math.floor(countUniq / 3); // give discount 5%2 = 1 , 5/2 = 2
          allDiscount = price * discount * 0.11; // discount 11 %
        }
        if (allHarry.length == 4) {
          let price = 0;
          allHarry.forEach(i => {
            price = parseInt(parseInt(price) + parseInt(i.price));
            countUniq = countUniq + i.qty;
          });
          let discount = Math.floor(countUniq / 4); // give discount 5%2 = 1 , 5/2 = 2
          allDiscount = price * discount * 0.12; // discount 11 %
        }
        if (allHarry.length == 5) {
          let price = 0;
          allHarry.forEach(i => {
            price = parseInt(parseInt(price) + parseInt(i.price));
            countUniq = countUniq + i.qty;
          });
          let discount = Math.floor(countUniq / 5); // give discount 5%2 = 1 , 5/2 = 2
          allDiscount = price * discount * 0.13; // discount 11 %
        }
        if (allHarry.length == 6) {
          let price = 0;
          allHarry.forEach(i => {
            price = parseInt(parseInt(price) + parseInt(i.price));
            countUniq = countUniq + i.qty;
          });
          let discount = Math.floor(countUniq / 6); // give discount 5%2 = 1 , 5/2 = 2
          allDiscount = price * discount * 0.14; // discount 11 %
        }
        if (allHarry.length == 7) {
          let price = 0;
          allHarry.forEach(i => {
            price = parseInt(parseInt(price) + parseInt(i.price));
            countUniq = countUniq + i.qty;
          });
          let discount = Math.floor(countUniq / 7); // give discount 5%2 = 1 , 5/2 = 2
          allDiscount = price * discount * 0.15; // discount 11 %
        }
        return allDiscount;

      },


    },
  };
</script>
<style scoped="">
  .background-body {
    background-color: #f8f8f8;
  }

  .cursor {
    cursor: pointer
  }

  .cursor :hover {
    cursor: pointer;
    font-size: 20px;
    color: #D50000 !important;
  }

  .img {
    max-width: 170px;
    width: 100%;
    height: 250px;
    display: block;
    margin: auto;
  }

  .img-card {
    max-width: 30px;
    width: 100%;
    height: 60px;
    display: block;
    margin: auto;
  }

  .snack {
    margin-right: 0px !important;
    padding: 0px !important;

  }

  .alert {
    margin-bottom: 0px !important;
    padding: 0px !important;
  }

  @media (max-width: 600px) {
    p {
      font-size: 10px !important;
    }

    .icon {
      font-size: 12px !important;
    }

    .text-company {
      font-size: 15px;
    }

    h4 {
      font-size: 14px;
    }
  }
</style>