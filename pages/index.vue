<template>
  <!-- create -->
  <div class="p-7">
    <form @submit.prevent="add(form)">
      <div class="grid grid-cols-5 gap-2">
        <input
          type="text"
          class="border-2 rounded w-full"
          v-model="form.name"
        />
        <input
          type="text"
          class="border-2 rounded w-full"
          v-model="form.price"
        />
        <input
          type="text"
          class="border-2 rounded w-full"
          v-model="form.description"
        />
        <input
          type="text"
          class="border-2 rounded w-full"
          v-model="form.quantity"
        />
        <button type="submit" class="bg-green-300 rounded w-full">Add</button>
      </div>
    </form>
  </div>
  <!-- update delete view -->
  <div class="grid gap-2 p-7">
    <div v-for="data in data.data" :key="data.id">
      <form @submit.prevent="submit(data.id, data.attributes)">
        <div class="grid grid-cols-5 gap-2">
          <input
            type="text"
            class="border-2 rounded w-full"
            v-model="data.attributes.name"
          />
          <input
            type="text"
            class="border-2 rounded w-full"
            v-model="data.attributes.price"
          />
          <input
            type="text"
            class="border-2 rounded w-full"
            v-model="data.attributes.description"
          />
          <input
            type="text"
            class="border-2 rounded w-full"
            v-model="data.attributes.quantity"
          />
          <div class="flex gap-2">
            <button type="submit" class="bg-green-300 rounded w-full">
              Update
            </button>
            <div
              class="bg-red-300 rounded w-full text-center cursor-pointer"
              @click="removeProduct(data.id)"
            >
              Delete
            </div>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>
<script setup>
const { find, update, delete: remove, create } = useStrapi();
const form = ref({
  name: "",
  price: 0,
  description: "",
  quantity: 0,
});
const data = ref("");
const getData = async () => {
  await find("products").then((res) => {
    data.value = res;
  });
};
onMounted(async () => {
  getData();
});
const submit = async (id, data) => {
  await update("products", id, data).then((res) => {
    console.log(res);
    getData();
  });
};
const removeProduct = async (id) => {
  await remove("products", id).then((res) => {
    console.log(res);
    getData();
  });
};
const add = async (data) => {
  await create("products", data).then((res) => {
    console.log(res);
    getData();

    // clear
    form.value.name = "";
    form.value.price = 0;
    form.value.description = "";
    form.value.quantity = 0;
  });
};
</script>
