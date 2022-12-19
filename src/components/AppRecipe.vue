

<template>
  <div class="background">
    
    <div class="container" >
      <div class="row">
        <div class="col-sm-12">
         
          
          <h1>Delicious Recipes for Any Occasion</h1>
          <hr />
          
          <!-- Allert Message -->
          <b-alert v-if="showMessage" variant="success" show>{{ message }}</b-alert>
          
          <!-- Create Recipe Button -->
          <button type="button" class="btn btn-primary btn-lg p-3" v-b-modal.recipe-modal ><b-icon icon="plus"></b-icon>Create</button>
          
          <!-- Recipe Cards -->
          
          <div class="row">
            <div class="col-sm-6" v-for="recipe in recipes" :key="recipe.id">
              <div class="card">
                <div class="card-body">
                <h5 class="card-title">{{ recipe.name }}</h5>
                <p class="card-text">Ingredients: {{ recipe.ingr }}</p>
                <p class="card-text">Instructions: {{ recipe.inst }}</p>
                <div class="d-flex justify-content-between align-items-center">
                  <!-- Star Rating -->
                  <div>
                    <b-icon 
                                  v-if="recipe.star == true" icon="heart-fill">
                                  </b-icon>
                                  <b-icon v-if="(recipe.star == false)" icon="heart">
                                  </b-icon>
                  </div>
                  <!-- Recipe Rating -->
                  <div>
                    <b-form-rating v-model="recipe.rate" readonly variant="warning" class="mb-2"></b-form-rating>
                    <p class="mt-2"></p>
                  </div>
                </div>
                <!-- Edit and Delete Buttons -->
                <div class="btn-group" role="group">
                  <button type="button" class="btn btn-info btn-sm" v-b-modal.edit-recipe-modal @click="editRecipe(recipe)">Edit</button>
                  <button type="button" class="btn btn-danger btn-sm" @click="deleteRecipe(recipe)">Delete</button>
                </div>
              </div>
              </div>
              </div>
            </div>
        
          
          <footer class="text-center">
          </footer>
        </div>
      </div>
      </div>
      
      <!-- Beginning of Modal for Create -->
      <div>  
        <b-modal ref="addRecipeModal" id="recipe-modal" title="Add Recipe" hide-backdrop hide-footer>
          <!-- Add Recipe Form -->
          <b-form @submit="onSubmit" class="w-100">
        <b-form-group id="name" label="Recipe Name:" label-for="name" description="Name of the recipe.">
          <b-form-input id="name" type="text" v-model="createRecipeForm.name" placeholder="name" required></b-form-input>
        </b-form-group>
        <b-form-group id="ingr" label="Ingredients:" label-for="ingr" description="List of ingredients, separated by commas.">
          <b-form-textarea id="ingr" v-model="createRecipeForm.ingr" placeholder="ingredients" required></b-form-textarea>
        </b-form-group>
        <b-form-group id="inst" label="Instructions:" label-for="inst" description="Step-by-step instructions for preparing the recipe.">
          <b-form-textarea id="inst" v-model="createRecipeForm.inst" placeholder="instructions" required></b-form-textarea>
        </b-form-group>  
        <b-form-group id="star" label="Add to Favorites:" label-for="star" description="Add the recipe to your favorites list.">
          <b-form-checkbox id="star" v-model="createRecipeForm.star"></b-form-checkbox>
        </b-form-group>
        <b-form-group id="rate" label="Rating:" label-for="rate" description="Rate the recipe (1 = worst, 5 = best).">
          <b-form-rating id="rate" v-model="createRecipeForm.rate" variant="warning"></b-form-rating>
        </b-form-group>
        <!-- Submit Button -->
            <b-button type="submit" variant="primary">Add Recipe</b-button>
          </b-form>
        </b-modal>
      
      
      <!-- Beginning of Modal for Edit -->
      <div>
        <b-modal ref="editRecipeModal" id="edit-recipe-modal" title="Edit Recipe" hide-backdrop hide-footer>
          <!-- Edit Recipe Form -->
          <b-form @submit="onSubmitUpdate" class="w-100">
            <b-form-group id="name" label="Recipe Name:" label-for="name" description="Name of the recipe.">
              <b-form-input id="name" type="text" v-model="editRecipeForm.name" placeholder="name" required></b-form-input>
            </b-form-group>
            <b-form-group id="ingr" label="Ingredients:" label-for="ingr" description="List of ingredients separated by commas.">
          <b-form-textarea id="ingr" v-model="editRecipeForm.ingr" placeholder="ingredients" required></b-form-textarea>
        </b-form-group>
        <b-form-group id="inst" label="Instructions:" label-for="inst" description="Step-by-step instructions for preparing the recipe.">
          <b-form-textarea id="inst" v-model="editRecipeForm.inst" placeholder="instructions" required></b-form-textarea>
        </b-form-group>
        <b-form-group id="star" label="Add to Favorites:" label-for="star" description="Add the recipe to your favorites list.">
          <b-form-checkbox id="star" v-model="editRecipeForm.star"></b-form-checkbox>
        </b-form-group>
        <b-form-group id="rate" label="Rating:" label-for="rate" description="Rate the recipe (1 = worst, 5 = best).">
          <b-form-rating id="rate" v-model="editRecipeForm.rate" variant="warning"></b-form-rating>
        </b-form-group><!-- Submit Button -->
            <b-button type="submit" variant="primary">Edit Recipe</b-button>
          </b-form>
        </b-modal>
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
  
            editRecipeForm: {
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
              name: this.editRecipeForm.name,
              ingr: this.editRecipeForm.ingr,
              inst: this.editRecipeForm.inst,
              star: this.editRecipeForm.star,
              rate: this.editRecipeForm.rate
          };
          this.RESTupdateRecipe(payload, this.editRecipeForm.id);
          this.initForm();
      },
  
        deleteRecipe(recipe) {
            this.RESTdeleteRecipe(recipe.id);
        },
  
        editRecipe(recipe) {
            this.editRecipeForm = recipe;
        },
  
        
  
    },
   
  
    created() {
        this.RESTgetRecipes();
    },
  };
  
  </script>
<style>

    


h1 {
  font-family: Arial, sans-serif;
  color: rgb(255, 246, 220);
  justify-content: center;
  align-items: center;
}
@keyframes fade-in {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
.card-top{
  size:cover;
}
.card {
    background-color: #e0dabf;
    border-radius: 6px;
    display: inline-block;
    width: 90%;
  }
  .card-title {
    font-size: 18px;
  }
  .card:hover {
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    cursor: pointer;
  }
  .btn-primary {
    background-color: #d4a303cc;
  }
  .btn-primary:hover {
    background-color: #d9cb00;
  }
  .btn-group {
    color: #9c810a;
  }
  .btn-group:hover {
    color: #8f7606;
  }
  .btn-danger {
    background-color: #dc3545;
  }
  .btn-danger:hover {
    background-color: #c82333;
  }


</style>

