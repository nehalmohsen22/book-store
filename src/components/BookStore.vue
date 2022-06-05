<style scoped>
.less {
  color: cadetblue;
}
.more {
  color: blueviolet;
}
</style>
<template>
  <div class="container" id="app">
    <div
      class="d-flex justify-content-between align-itmes-baseline bg-dark text-light mt-2 px-2"
    >
      <a href="#" class="text-warning" @click.prevent="islistvisible = false"
        >book</a
      >
      <div class="d-flex align-items-baseline">
        <p>
          {{ booklist.length }}
          <span v-if="booklist.length > 1">Books </span>
          <span v-else>Book </span>
        </p>
        <span> | With Total Price: {{ handleCurrency(getTotalPrice()) }}</span>
        <button class="btn btn-primary mx-2" @click="islistvisible = true">
          Show List
        </button>
      </div>
    </div>
    <!--books-->
    <div class="row justify-content-center mb-2" v-if="islistvisible == false">
      <div
        v-for="book in books"
        :key="book.id"
        class="card"
        style="width: 23rem"
      >
        <img
          src="../assets/book.webp"
          :title="book.title"
          class="card-img-top"
        />
        <h5 class="text-center my-2">Author: {{ book.author }}</h5>
        <p style="text-align: justify">Language: {{ book.language }}</p>
        <p style="text-align: justify">Year: {{ book.year }}</p>
        <p style="text-align: justify">
          Price: {{ handleCurrency(book.price) }}
        </p>

        <div
          class="card-footer d-flex justify-content-between align-items-baseline"
        >
          <p
            :class="[
              book.pages >= 500 ? 'more' : '',
              book.pages < 500 ? 'less' : '',
            ]"
          >
            Pages <span>{{ book.pages }}</span>
          </p>

          <button
            class="btn btn-primary"
            :disabled="checkExist(book.id)"
            @click="addToList(book)"
          >
            Add to wish list
          </button>
        </div>
      </div>
    </div>
    <!--end-->
    <div class="row" v-if="islistvisible == true">
      <h4 class="text-center text-danger" v-if="booklist.length == 0">
        Sorry, cart is empty add books
      </h4>
    </div>
    <div class="watchList" v-if="islistvisible == true">
      <div class="table-responsive" v-if="booklist.length > 0">
        <table
          class="table table-hover table-bordered table-striped text-center"
        >
          <thead>
            <tr>
              <th>Book Name</th>
              <th>Author</th>
              <th>Pages</th>
              <th>Price</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in booklist" :key="item">
              <td>{{ item.book.title }}</td>
              <td>{{ item.book.author }}</td>
              <td>{{ item.book.pages }}</td>
              <td>{{ handleCurrency(item.book.price) }}</td>
              <td>
                <button
                  class="btn btn-danger"
                  type="button"
                  @click="removeFromList(item)"
                >
                  -
                </button>
              </td>
            </tr>
          </tbody>
          <tfoot>
            <tr>
              <th colspan="3">Total Price:</th>
              <td colspan="3">{{ handleCurrency(getTotalPrice()) }}</td>
            </tr>
          </tfoot>
        </table>
      </div>
    </div>
  </div>
</template>
<script>
import books from "../books";
export default {
  data: () => ({
    books: books,
    islistvisible: false,
    booklist: [],
  }),
  methods: {
    addToList(book) {
      this.booklist.push({
        book: book,
      });
    },
    getTotalPrice() {
      let total = 0;
      for (let item of this.booklist) {
        total += item.book.price;
      }
      return total;
    },
    handleCurrency(value) {
      return new Intl.NumberFormat("en-US", {
        style: "currency",
        currency: "USD",
      }).format(value);
    },
    removeFromList(book) {
      this.booklist.splice(
        this.booklist.findIndex((item) => item.book.id == book.book.id),
        1
      );
    },
    checkExist(book_id) {
      return this.booklist.some((item) => item.book.id == book_id);
    },
  },
  computed: {},
  components: {},
};
</script>
