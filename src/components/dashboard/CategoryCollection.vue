<template>
  <div class="container-fluid">
    <div class="row">

      <div class="col-lg-6 py-5 my-5 px-5">
        <form>
          <div class="">
            <label class="form-label" for="form12">Category Name <span class="text-danger">*</span></label>
            <input type="text" id="form12" class="form-control" v-model="category" required />

            <div class="mt-3 text-end">
              <button class="btn btn-sm btn-primary" @click.prevent="updateStore">Add Category</button>
            </div>
          </div>
        </form>

      </div>

      <div class="col-lg-6 vh-100 py-5" style="background-color: #f1f3f4;">
        <div Dividerlass="h4 pb-5">Categories</div>
        <div class="row">
          <div class="col-lg-6 justify-content-between py-2" v-for="(category, index) in categoryStore.categories"
            :key="index">
            <div class="card">
              <div class="card-body py-1 px-3 d-flex justify-content-between align-items-center">
                <div class="small fw-bold">{{ category['name'] }}</div>
                <a class="text-danger nav-link btn shadow-0" style="curosr:pointer;" @click.prevent="deleteCategory(category.slug)">X</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>


  </div>
</template>


<script setup>
import { ref, onMounted, computed } from 'vue';
import { useblogCategory } from '../../store/blogCategory';
import { useRoute } from 'vue-router';
import router from '../../router';


const categoryStore = useblogCategory();
const category = ref('');
const route= useRoute();


function updateStore() {
  let categoryData = {
    'name': category.value
  }
  if (categoryData.name == "") {
    alert('Please enter the category name.');
    return false;
  }
  category.value = ""
  categoryStore.addCategory(categoryData);

}

// const id = computed(() => route.params.id);
async function deleteCategory(slug) {
  await categoryStore.deleteCategory(slug);
}

onMounted(async () => {
  categoryStore.readAllCategory();
});


computed(() => {
  return categoryStore.categories;
});


</script>



<style scoped>
input {
  height: 4rem;
  border: 3px solid #f1f1f1 !important;
  border-radius: 0.7rem;
  padding: 0.5rem 1rem !important;
  font-size: 1.2rem !important;
}

input:focus:hover,
input:focus {
  border: 3px solid #084e88 !important;
}

input:hover {
  border: 3px solid #ccc !important;
  transition: 0.7s;
}

label {
  font-weight: 800;
}
</style>