<template>
  <div class="home">
    <table>
      <tr>
        <th>ID</th>
        <th>Name</th>
        <th>Value</th>
        <th>Change</th>
      </tr>
      <tr v-for="(item, index) in array" v-bind:key="index">
        <td>{{ item.id }}</td>
        <td>{{ item.name }}</td>
        <td>${{ item.price }}</td>
        <td class="hey">
          <button @click="deleteProduct(index, 1)">
            <i class="fas fa-times-circle"></i>
          </button>
          <button @click="handleEditClick(item)">
            <i class="fas fa-edit"></i>
          </button>
        </td>
      </tr>
    </table>

    <div class="model-container">
      <button class="openModelBtn" @click="openModel = true">
        Add Product
      </button>
      <transition name="fade" appear>
        <div class="model-overlay" v-if="openModel"></div>
      </transition>
      <transition class="trans" name="slide" appear>
        <div class="model" v-if="openModel">
          <form @submit.prevent="addNewProduct">
            <h3>Add New Item To Cart</h3>
            <input
              v-model="name"
              type="text"
              placeholder="Product Name"
              required
            />
            <input v-model="price" type="text" placeholder="Price" required />

            <button class="close-button" @click="openModel = false">
              Cancel
            </button>
            <button type="submit" class="add-button" @click="openModel = false">
              {{ editable ? "update item" : "Add Item" }}
            </button>
          </form>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      openModel: false,
      editable: false,
      selectedItem: null,
      name: "",
      price: "",

      array: [
        {
          id: 0,
          name: " Moisturizer",
          price: "50"
        },
        {
          id: 1,
          name: "  Marker",
          price: "20"
        },
        {
          id: 2,
          name: " Shoe Polish",
          price: "120"
        },
        {
          id: 3,
          name: " Hand Bag",
          price: "500"
        }
      ]
    };
  },
  methods: {
    addNewProduct() {
      if (!this.editable) {
        this.array.push({
          id: this.array.length,
          name: this.name,
          price: this.price
        });
        console.log("add", this.array);
      } else {
        this.array.splice(this.selectedItem.id, 1, {
          id: this.selectedItem.id,
          name: this.name,
          price: this.price
        });
        this.editable = false;
        console.log("edit", this.array);
      }
      (this.name = ""), (this.price = "");
    },
    deleteProduct(index) {
      this.array.splice(index, 1);
    },
    handleEditClick(item) {
      this.openModel = true;
      this.name = item.name;
      this.price = item.price;
      this.editable = true;
      this.selectedItem = item;
    },
    editProduct(index) {
      this.array.push(this.item(index, 1));
    }
  }
};
</script>
<style scoped>
table {
  margin: auto;
  width: 70%;
  height: 70%;
  border: 1px rgb(53, 55, 56) solid;
}
th {
  color: aliceblue;
  height: 28px;
  border-bottom: 3px rgb(53, 55, 56) solid;
  background-color: rgb(53, 55, 56);
}
td {
  height: 26px;
  border-left: 1px rgb(53, 55, 56) solid;
  border-right: 1px rgb(53, 55, 56) solid;
}

transition {
  border: solid 1px rgb(53, 55, 56);
  background-color: rgb(53, 55, 56);
}
.model-overlay {
  /* width: 100%; */
  height: 100%;
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  background-color: rgba(4, 0, 0, 0.878);
  z-index: 9;
}
.model {
  position: fixed;
  top: 16%;
  right: 34%;
  padding-top: 2em;
  z-index: 10;
  width: 410px;
  height: 340px;
  background-color: rgba(0, 0, 0, 0.4);
  border: rgb(116, 120, 122) 1px solid;
}
.model input {
  font-size: 20px;
  margin: auto;
  margin-top: 1em;
  height: 40px;
  width: 90%;
  outline: none;
  border-radius: 6px;
  display: block;
}
.model button {
  margin: auto;
  margin-top: 1em;
  height: 40px;
  width: 90%;
  outline: none;
  border-radius: 6px;
  background-color: rgb(53, 55, 56);
  cursor: pointer;
  font-size: 20px;
}
form h3 {
  margin: auto;
  color: white;
  background-color: rgb(53, 55, 56);
  width: 80%;
  height: 40px;
  padding-top: 15px;
  margin-top: 4px;
}
button {
  border: none;
  margin: auto;
  color: white;
  margin-top: 1em;
  height: 40px;
  width: 200px;
  outline: none;
  border-radius: 6px;
  background-color: rgb(53, 55, 56);
  cursor: pointer;
  font-size: 20px;
  box-shadow: 3px 3px rgba(0, 0, 0, 0.3);
  transition: 0.2s ease-in-out;
}
button:hover {
  background-color: #2e7957;
  font-size: 16px;
}
.hey button {
  width: 40px;
  margin: 4px;
}
</style>
