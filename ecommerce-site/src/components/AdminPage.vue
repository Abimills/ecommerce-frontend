<template>
  <div class="create-product-container">
    <div class="admin-mother-container">
      <div class="admin-information-navigation">
        <div class="admin-pic-label">
          <img src="../assets/cutepie23.png" alt="" class="admin-pic" />
          <p>Admin</p>
        </div>
        <div class="profile-container">
          <p>Profile</p>
        </div>
        <div class="categories-container">
          <p>Categories</p>
        </div>
        <div class="Add-product-container">
          <p>Add Products</p>
        </div>
        <div class="view-products-container">
          <p>View Products</p>
        </div>
        <div class="user-request-container">
          <p>View User</p>
        </div>
        <div class="log-out-request-container">
          <button>Log out</button>
        </div>
      </div>
    </div>
    <!-- <div class="add-products-form-container">
      <h1 class="add-products-header">Add Products</h1>
      <div class="line"></div>
      <form class="add-product-form" @submit.prevent="onSubmit">
        <div class="name-container">
          <label class="label">Product Name</label>
          <input
            type="text"
            class="name"
            placeholder="Enter Product name"
            v-model="data.name"
          />
        </div>
        <div class="price-container">
          <label class="label">Product Price</label>
          <input
            type="number"
            class="name"
            placeholder="Enter Product price"
            v-model="data.price"
          />
        </div>
        <div class="category-container">
          <label class="label">Category</label>
          <input
            type="text"
            class="name"
            placeholder="Enter Product category"
            v-model="data.category"
          />
        </div>
        <div class="description-container">
          <label class="label">Product Description</label>
          <input
            type="text"
            class="name"
            placeholder="Enter Product Description"
            v-model="data.description"
          />
        </div>
        <div class="discount-container">
          <label class="label"> Discount </label>
          <input
            type="number"
            class="name"
            placeholder="Enter Product discount if applies"
            v-model="data.discount"
          />
        </div>
        <div class="time-ranges-container">
          <label class="label">
            Give label for the product entrance name
          </label>
          <input
            type="text"
            class="name"
            placeholder="Label product entrance time"
            v-model="data.timeRanges"
          />
        </div>
        <div class="colors-container">
          <label class="label">Product colors </label>
          <input
            type="text"
            class="name"
            placeholder="Enter Product colors availability"
            v-model="data.colors"
          />
        </div>
        <div class="img-container">
          <label class="label">Product Image </label>
          <input
            type="file"
            class="name"
            placeholder="Provide product image"
            @change="handleImage"
          />
        </div>

        <input type="submit" class="submit-btn" value="Add Product" />
      </form>
    </div> -->
    <!-- ========================================== end of add product -->
    <!-- <div class="profile-all-container">
      <div class="profile-all">
        <div class="top-img-info-container">
          <img
            src="../assets/cutepie23.png"
            alt=""
            class="person-profile-img"
          />
          <div class="person-info">
            <p>Abel Miller.</p>
            <p>juli@gmail.com</p>
            <p class="owner">Owner</p>
          </div>
        </div>
        <div class="product-info-container">
          <ul class="list-info">
            <div class="li-container">
              <li class="list">categories of products</li>
              <p>3+</p>
            </div>
            <div class="li-container">
              <li class="list">Newsletter subscribers</li>
              <p>20+</p>
            </div>
            <div class="li-container">
              <li class="list">Reviews</li>
              <p>20+</p>
            </div>
            <div class="li-container">
              <li class="list">users</li>
              <p>20+</p>
            </div>
            <div class="li-container">
              <li class="list">products</li>
              <p>100+</p>
            </div>
          </ul>
        </div>
      </div>
    </div> -->
    <!-- =========================end of profile -->

    <!-- <div class="new-products-container">
      <h1 class="brand-header">Categories</h1>
      <div class="category-container">
        <p
          class="filter"
          :class="activeCategory == cat ? 'active-category' : ''"
          @click="filterProducts(cat, index)"
          v-for="cat in productCategory?.categories"
          :key="cat.index"
        >
          {{ cat }}
        </p>
      </div>

      <div class="product-container">
        <div
          class="product"
          v-for="product in productsToShow?.product?.products"
          :key="product?._id"
        >
          <img :src="product?.img" alt="" class="product-img" />
          <div class="price-container">
            <router-link to="'/product/' + product?._id">
              <p class="product-name">{{ product?.name }}</p>
            </router-link>
            <p class="product-price">${{ product?.price }}</p>
          </div>
          <p class="description">
            {{ product?.description }}
          </p>
          <button class="add-to-cart">Add to Cart</button>
        </div>
      </div>
    </div> -->
    <!-- ============== category page ended -->
    <SingleUser />
  </div>
</template>

<script setup>
import axios from "axios";
import SingleViewVue from "./SingleView.vue";
import SingleUser from "./SingleUser.vue";
import { onMounted, reactive, ref } from "vue";
const showPage = ref("singleUser");
const activeCategory = ref("Shoes");
const data = reactive({
  name: "",
  price: "",
  description: "",
  category: "",
  img: "3241324",
  colors: "",
  timeRanges: "",
  discount: "",
});
// categories set of words from data
const productCategory = reactive({ categories: [] });

const handleImage = (e) => {
  const file = e.target.files[0];
  data.img = file;
};
const products = reactive({ product: [] });
const productsToShow = reactive({
  product: [],
});
const getCategories = (data) => {
  const categories = data?.products?.map((pro) => pro.category);
  const timeRangeCat = data?.products?.map((pro) => pro.timeRanges[0]);
  productCategory.categories = [
    "All",
    ...new Set(categories),
    ...new Set(timeRangeCat),
  ];


};

onMounted(() => {
  const fetch = async () => {
    const res = await axios.get("https://my-ecommerce-bkends.onrender.com/products/");
    // productsToShow.product = res.data;
    products.product = res.data;
    filterProducts("Shoes");
    getCategories(res.data);
  };
  fetch();
});

const onSubmit = async () => {
  if (
    data?.price &&
    data?.name &&
    data?.description &&
    data?.category &&
    data?.img &&
    data?.timeRanges &&
    data?.img
  ) {
    const newData = new FormData();
    newData.append("file", data?.img);
    newData.append("upload_preset", "uploads");
    const imgUploaded = await axios.post(
      "https://api.cloudinary.com/v1_1/dnokvmwmd/image/upload",
      newData
    );
    const { url } = imgUploaded.data;
    const res = await axios.post(`https://my-ecommerce-bkends.onrender.com/products/`, {
      ...data,
      img: url,
    });
   
  } else {
    alert("Please provide stared fields");
  }
};

const filterProducts = (category) => {
  if (products.product) {
    if (category === "All") {
      productsToShow.product.products = products.product.products;
      activeCategory.value = category;
    } else {
      productsToShow.product.products = products.product.products?.filter(
        (product) =>
          product.category?.toLowerCase() == category?.toLowerCase() ||
          product.timeRanges[0]?.toLowerCase() == category?.toLowerCase()
      );
      activeCategory.value = category;
    }
  } else {
    return;
  }
};

</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Caveat:wght@400;500;600;700&family=Croissant+One&family=Dosis:wght@200;300;400;500;600;700;800&family=Mooli&family=Outfit:wght@100;200;300;400;500;600;700;800;900&family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap");
/* font-family: 'Caveat', cursive; */
/* font-family: 'Croissant One', cursive; */
/* font-family: 'Dosis', sans-serif; */
/* font-family: 'Mooli', sans-serif; */
/* font-family: 'Outfit', sans-serif; */
/* font-family: 'Roboto', sans-serif; */
.create-product-container {
  width: 100%;
  display: flex;
  /* align-items: center; */
  justify-content: space-between;
  gap: 6rem;
  position: relative;

  font-family: "Outfit", sans-serif;
}
.right-side-container {
  width: 100%;
}
.add-products-form-container {
  width: 70%;
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  flex-direction: column;
  gap: 1rem;
  margin-left: 14rem;
}
.log-out-request-container {
  margin-top: 1rem;
  width: 100%;
}
.log-out-request-container button {
  width: 100%;
  background: #90cbc9;
  padding: 10px;
  border: 1px solid #90cbc9;
  color: white;
  text-transform: capitalize;
}
.log-out-request-container button:hover {
  background: transparent;
  color: black;
}
.add-product-form {
  width: 60%;

  padding: 20px;
  border: 1px solid #90cbc9;
  border-radius: 5px;
}
.name-container,
.price-container,
.img-container,
.colors-container,
.time-ranges-container,
.discount-container,
.description-container,
.category-container {
  display: flex;
  align-items: flex-start;
  width: 100%;
  flex-direction: column;
  justify-content: flex-start;
}
.name-container label,
.price-container label,
.img-container label,
.colors-container label,
.time-ranges-container label,
.discount-container label,
.description-container label,
.category-container label {
  color: #90cbc9;
  font-size: 0.8rem;
  margin-bottom: 0.4rem;
}
.name-container input::placeholder,
.price-container input::placeholder,
.img-container input::placeholder,
.colors-container input::placeholder,
.time-ranges-container input::placeholder,
.discount-container input::placeholder,
.description-container input::placeholder,
.category-container input::placeholder {
  font-size: 0.6rem;
  color: #90cbc9;
}
.name-container input,
.price-container input,
.img-container input,
.colors-container input,
.time-ranges-container input,
.discount-container input,
.description-container input,
.category-container input {
  width: 100%;
  padding: 10px;
  border: 1px solid white;
  outline: none;
  margin-bottom: 1rem;
}
.submit-btn {
  width: 100%;
  padding: 10px;
  background-color: #90cbc9;
  border: 1px solid #90cbc9;
  color: white;
  outline: none;
  cursor: pointer;
}
.add-products-header {
  color: white;
  font-size: 2rem;
  font-family: "Roboto", sans-serif;
  letter-spacing: 2px;
  margin-bottom: -1rem;
}
.line {
  content: none;
  width: 200px;
  height: 2px;
  background: #90cbc9;
  border-radius: 40px;
  /* margin-bottom: 1rem; */
}
.admin-pic {
  width: 70px;
  height: 70px;
  object-fit: contain;
  border-radius: 50%;
  background: orange;
  display: flex;
  align-items: center;
  justify-content: center;
}

.admin-mother-container {
  width: 25%;
  height: 100vh;
  position: relative;
}
.admin-information-navigation {
  width: 20%;
  height: 100vh;
  left: 0;
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: flex-start;
  background-color: white;
  /* margin-left: 3rem; */
  border-top-right-radius: 90px;
  position: fixed;

  padding: 10px;
}
.admin-pic-label {
  width: 100%;
  display: flex;
  align-items: center;

  justify-content: space-evenly;
  margin: 1rem;
  border-bottom: 1px solid rgb(184, 183, 183);
  padding: 20px 10px;
}
.admin-pic-label p {
  /* font-family: "Roboto", sans-serif; */
  font-weight: 600;
  color: #064240;
}
.profile-container,
.categories-container,
.Add-product-container,
.view-products-container,
.user-request-container {
  width: 100%;
  margin: 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px;
  border-bottom: 1px solid #90cbc9;
}
.profile-container p,
.categories-container p,
.Add-product-container p,
.view-products-container p,
.user-request-container p {
  color: #064240;
  color: #90cbc9;
  cursor: pointer;
}
.person-profile-img {
  width: 150px;
  height: 150px;
  object-fit: cover;
  background: rgb(20, 20, 20);
  border-radius: 50%;
  padding: 10px;
}
.profile-all-container {
  /* margin-left: 15rem; */
  /* margin-left: 16rem; */
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  /* flex-direction: column; */
  margin-left: 4rem;
  margin-top: 2rem;
}
.profile-all {
  width: 50%;
  border: 1px solid #90cbc9;
  padding: 10px;
  border-radius: 5px;
}
.person-info {
  color: white;
  display: flex;
  align-items: flex-start;
  flex-direction: column;
  justify-content: flex-start;
  gap: 0.5rem;
  font-size: 0.8rem;
}

.top-img-info-container {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
  padding: 10px;
  border-bottom: 1px solid gray;
}
.product-info-container {
  width: 100%;
  background: white;
  text-transform: capitalize;
}
.list-info {
  padding: 10px;
  display: flex;
  align-items: center;
  flex-direction: column;
  gap: 1rem;
}
li {
  list-style: none;
}
.li-container {
  width: 100%;
  display: flex;
  justify-content: space-between;
  background: aliceblue;
  padding: 10px;
  /* margin:.5rem; */
  color: #064240;
}
.li-container p {
  background: white;
  padding: 5px;
}
.owner {
  padding: 3px 10px;
  background: #90cbc9;
}
.product-img {
  width: 150px;
  height: 150px;
  object-fit: contain;
}
.new-products-container {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  margin-left: 15rem;
  margin-top: 6rem;
  margin-bottom: 6rem;
}
.brand-header {
  color: rgb(217, 217, 217);
  font-family: "Dosis", sans-serif;
  font-weight: 400;
  text-transform: uppercase;
  margin-bottom: 2rem;
}
.brand-text {
  color: rgb(159, 156, 156);
  font-family: "Dosis", sans-serif;
  font-weight: 400;
  text-transform: uppercase;
  font-size: 0.7rem;
  margin-bottom: 2rem;
}
.product-container {
  width: 100%;
  display: flex;
  justify-content: space-evenly;
  gap: 2rem;
  align-items: center;
  flex-wrap: wrap;
}
.product {
  width: 250px;
  padding: 10px;
  background: #709290;
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
}
.price-container {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 1rem;
}
.product-name {
  font-family: "Dosis", sans-serif;
  color: white;
}
.product-price {
  font-family: "Dosis", sans-serif;
  color: white;
  background-color: #064240;
  padding: 0px 8px;
}
.description {
  font-family: "Dosis", sans-serif;
  color: white;
  margin-bottom: 1rem;
  font-size: 0.8rem;
}
.add-to-cart {
  padding: 5px 15px;
  background: #064240;
  border: 1px solid #064240;
  color: white;
  border-radius: 20px;
  font-family: "Dosis", sans-serif;
  margin: 0.5rem 0;
  font-size: 0.7rem;
  transition: all 0.4s ease-in;
}
.add-to-cart:hover {
  background: transparent;
}
.category-container {
  width: 80%;
  margin-left: 8rem;
  display: flex;
  flex-direction: row;
  align-items: center;
  background: white;
  gap: 2rem;
  justify-content: space-evenly;
  padding: 10px;
  margin: 0 auto;
  margin-bottom: 1rem;
  border-radius: 20px;
}
.category-container p {
  cursor: pointer;
  transition: all 0.5s ease-in-out;
}
.active-category {
  background: orange;
  padding: 2px 10px;
  color: white;
  border-radius: 20px;
  transition: all 0.5s ease-in-out;
}
</style>