<template>
  <div class="jumbotron vertical-center">
      <div class = "container">
          <div class = "row">
              <div class = "col-sm-12">
                  <h1>Recipes</h1>
                  <hr />
                  <!-- Allert Message -->
        <b-alert v-if="showMessage" variant="success" show>{{
          message
        }}</b-alert>
        <!-- b-alert v-if="error" variant="danger" show>{{ error }}</b-alert-->

        <button
          type="button"
          class="btn btn-success btn-sm"
          v-b-modal.recipe-modal
        >
          Create 
        </button>
                  <br /> <br />
                  <table class = "table table-hover">
                      <thead>
                          <tr>
                              <th scope = "col"> Recipe Name </th>
                              <th scope = "col"> Recipe ingr </th>
                              <th scope = "col"> Recipe inst </th>
                              <th scope = "col"> Recipe star </th>
                              <th scope = "col"> Recipe rate </th>
                          </tr>
                      </thead>
                      <tbody>
                          <tr v-for= "recipe in recipes" :key = "recipe.id">
                              <td> {{ recipe.name }} </td>
                              <td> {{ recipe.ingr }} </td>
                              <td> {{ recipe.inst }} </td>
                              <td>
                                  <b-icon 
                                  v-if="recipe.star == true" icon="heart-fill">
                                  </b-icon>
                                  <b-icon v-if="(recipe.star == false)" icon="heart">
                                  </b-icon>
                              </td>
                                <td> 
                                    <div>
                                        <b-form-rating v-model= "recipe.rate" readonly variant="warning" class="mb-2"></b-form-rating>
                                        <p class="mt-2"></p>
                                    </div> 
                                 </td>
                              
                               <td> 
                                  <div class="btn-group" role="group">
                                      <button
                                      type="button"
                                      class="btn btn-info btn-sm"
                                      v-b-modal.edit-recipe-modal
                                      @click="editRecipe(recipe)"
                                      >
                                      Edit
                                      </button>
                                      <button
                                      type="button"
                                      class="btn btn-danger btn-sm"
                                      @click="deleteRecipe(recipe)"
                                      >
                                      Delete
                                      </button>
                                  </div>
                              </td>
                          </tr>
                      </tbody>
                  </table>
                  <footer class = "text-center">
                  </footer>
              
              </div>
          </div>
          <!-- Beginning of Modal for Create -->
              <div>   <b-modal 
              ref = "addRecipeModal"
              id="recipe-modal"
              title="Add Recipe"
              hide-backdrop
              hide-footer
              >
              <b-form @submit="onSubmit" class="w-100">
              <b-form-group
                  id="name"
              label="Recipe Name:"
              label-for="name"
              description="Name of the recipe."
              >
              <b-form-input
              id="name"
              type="text"
              v-model="createRecipeForm.name"
              placeholder="name"
              required
              >
              </b-form-input>
              </b-form-group>
              <b-form-group
              id="ingr"
              label="Recipe ingr:"
              label-for="ingr"
              description="ingr of the recipe."
              >
              <b-form-input
              id="ingr"
              type="text"
              v-model="createRecipeForm.ingr"
              placeholder="ingr"
              required
              ></b-form-input>
              </b-form-group>
              <b-form-group
              id="inst"
              label="Recipe inst:"
              label-for="inst"
              description="inst of the recipe."
              >
              <b-form-input
              id="inst"
              type="text"
              v-model="createRecipeForm.inst"
              placeholder="Recipe inst"
              required
              ></b-form-input>
              </b-form-group>
              <b-form-group
              id="form-star-group"    
              label="Recipe star:"
              label-for="form-edit-star-input"
              description="star this recipe"
              >
              <b-form-checkbox
              id="form-star-input"
              v-model="createRecipeForm.star"
              switch
              ></b-form-checkbox>
              </b-form-group>
              <b-form-group
              id="rate"
              label="Recipe rating:"
              label-for="form-rating-input"
              description="Rate this recipe from 1-5"
              >
              <b-form-rating
              id="rate"
              type="integer"
              v-model="createRecipeForm.rate"
              ></b-form-rating>
              </b-form-group>
              <b-button type="submit" variant="primary">Submit</b-button>
              </b-form>
          </b-modal>

          <!-- End of Modal for Create Recipe-->

          <!-- Beginning of Modal for Edit Recipe-->
          <b-modal
          ref = "editRecipeModal"
          id="edit-recipe-modal"
          title="Edit Recipe"
          hide-backdrop
          hide-footer
          >
          <b-form @submit="onSubmitUpdate" class="w-100">
          <b-form-group
          id="form-name-group"
          label="name:"
          label-for="form-name-input"
          description="Enter the name of the recipe."
          >
          <b-form-input
          id="form-name-input"
          type="text"
          v-model="updateRecipeForm.name"
          placeholder="name"
          
          >
          </b-form-input>
          </b-form-group>
          <b-form-group
          id="form-ingr-group"
          label="ingr:"
          label-for="form-ingr-input"
          description="Enter the ingr of the recipe."
          >
          <b-form-input
          id="form-ingr-input"
          type="text"
          v-model="updateRecipeForm.ingr"
          placeholder="ingr"
          >
          </b-form-input>
          </b-form-group>
          <b-form-group
          id="form-inst-group"
          label="Recipe inst:"
          label-for="form-inst-input"
          description="Enter the inst of the recipe."
          >
          <b-form-input

          id="form-inst-input"
          type="text"
          v-model="updateRecipeForm.inst"
          placeholder="Recipe inst"
          >
          </b-form-input>
          </b-form-group>

          <b-form-group

          id="form-star-group"
          label="Recipe star:"
          label-for="form-star-input"
          description="Let us know if you want to star this recipe"
          >
          <b-form-checkbox
          id="form-star-input"
          v-model="updateRecipeForm.star"
          switch
          ></b-form-checkbox>
          </b-form-group>
          <b-form-group
          id="form-rating-group"
          label="Recipe rating:"
          label-for="form-rating-input"
          description="Rate this recipe from 1-5"
          >
          <b-form-rating
          id="form-rating-input"
          type="integer"
          v-model="updateRecipeForm.rate"
          ></b-form-rating>
          </b-form-group>
  
          <b-button type="submit" variant="primary">Update</b-button>
          </b-form>
          </b-modal>
          <!-- End of Modal for Edit Recipe-->

      </div>
      
  
  </div>
</div>
 
  

</template>


<script>
  
  
  import axios from 'axios';
  export default
  {
    name: 'AppRecipes',
    data()
    {
        return {
            recipes: [],
            createRecipeForm: {
                name: '',
                ingr: '',
                inst: '',
                star: false,
                rate: 0
            },
  
            updateRecipeForm: {
                name: '',
                ingr: '',
                inst: '',
                star: false,
                rate: 0
            },
  
            showMessage: false,
            message: '',
  
        };
    },
    methods: {
  
      
      
        //GET Recipes
        RESTgetRecipes() {
        const path = `${process.env.VUE_APP_ROOT_URL}/recipes`;
        axios
            .get(path)
            .then((response) => {
                this.recipes = response.data.recipe;
                
                    // To actually show the message
                    this.showMessage = true;
                    // To hide the message after 5 seconds
                    setTimeout(() => {
                      this.showMessage = false;
                    }, 100000);
            }
            )
            .catch((error) => {
                console.log(error);
            });
        },
  
        //POST Recipes
        RESTcreateRecipe(payload){
            const path = `${process.env.VUE_APP_ROOT_URL}/recipe`;
            axios
                .post(path,payload)
                .then((response) => {
                    this.RESTgetRecipes();
                    this.message = "Recipe Created succesfully!";
                    // To actually show the message
                    this.showMessage = true;
                    // To hide the message after 5 seconds
                    setTimeout(() => {
                      this.showMessage = false;
                    }, 50000);
                })
                .catch((error) => {
                    console.log(error);
                    this.RESTgetRecipes()
                });
        },
  
        RESTupdateRecipe(payload, id){
            const path = `${process.env.VUE_APP_ROOT_URL}/recipe/${id}`;
            axios
                .put(path,payload)
                .then((response) => {
                    this.RESTgetRecipes();
  
                      this.message = "Recipe Updated Successfully"
                      this.showMessage = true;
                      setTimeout(() => {
                          this.showMessage = false;
                      }, 3000);
                      
                })
                .catch((error) => {
                    console.log(error);
                    this.RESTgetRecipes()
                });
        },
  
  
        RESTdeleteRecipe(id){
            const path = `${process.env.VUE_APP_ROOT_URL}/recipe/${id}`;
            axios
                .delete(path)
                .then((response) => {
                    this.RESTgetRecipes();
                    this.message = "Recipe Deleted Successfully"
                    this.showMessage = true;
                    set.TimeOut(() => {
                        this.showMessage = false;
                    }, 3000);
  
                })
                .catch((error) => {
                    console.log(error);
                    this.RESTgetRecipes()
                });
        },
  
        initForm(){
            this.createRecipeForm = {
                name: '',
                ingr: '',
                inst: '',
                star: false,
                rate: 0
            };
            this.updateRecipeForm = {
                name: '',
                ingr: '',
                inst: '',
                star: false,
                rate: 0
            };
  
        },
        onSubmit(e) {
            e.preventDefault(); 
            this.$refs.addRecipeModal.hide(); 
            const payload = {
                name: this.createRecipeForm.name,
                ingr: this.createRecipeForm.ingr,
                inst: this.createRecipeForm.inst,
                star: this.createRecipeForm.star,
                rate: this.createRecipeForm.rate
            };
            this.RESTcreateRecipe(payload);
            this.initForm();
        },
        onSubmitUpdate(e) {
          e.preventDefault(); 
          this.$refs.editRecipeModal.hide(); 
          const payload = {
              name: this.updateRecipeForm.name,
              ingr: this.updateRecipeForm.ingr,
              inst: this.updateRecipeForm.inst,
              star: this.updateRecipeForm.star,
              rate: this.updateRecipeForm.rate
          };
          this.RESTupdateRecipe(payload, this.updateRecipeForm.id);
          this.initForm();
      },
  
        deleteRecipe(recipe) {
            this.RESTdeleteRecipe(recipe.id);
        },
  
        editRecipe(recipe) {
            this.updateRecipeForm = recipe;
        },
  
        
  
    },
   
  
    created() {
        this.RESTgetRecipes();
    },
  };
  
  </script>
<style>
.vertical-center {
  min-height: 100%;  /* Fallback for browsers do NOT support vh unit */
  min-height: 100vh; /* These two lines are counted as one :-)       */

  display: flex;
  align-items: center;
}
</style>

