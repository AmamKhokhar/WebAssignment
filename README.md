<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Recipe Search</title>
</head>
<body>
  <h1>Recipe Search</h1>
  <label for="recipeName">Enter Recipe Name:</label>
  <input type="text" id="recipeName">
  <button onclick="searchRecipe()">Search</button>
  <div id="result"></div>

  <script>
    
    const recipes = [
      { name: 'Pasta Carbonara',
       ingredients: ['pasta', 'eggs', 'bacon', 'parmesan cheese'] },
      { name: 'Chicken Stir Fry',
       ingredients: ['chicken', 'vegetables', 'soy sauce'] },
      { name: 'Vegetarian Pizza',
       ingredients: ['dough', 'tomato sauce', 'cheese', 'vegetables'] },
    ];

    
    function searchRecipeByNameCallback(recipeName, callback) {
      setTimeout(() => {
        const result = recipes.find(recipe =>
          recipe.name.toLowerCase() === recipeName.toLowerCase()
        );
        callback(result ? null : 'Recipe not found.', result);
      }, 1000);
    }

   
    function searchRecipeByNamePromise(recipeName) {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          const result = recipes.find(recipe =>
            recipe.name.toLowerCase() === recipeName.toLowerCase()
          );
          if (result) {
            resolve(result);
          } else {
            reject('Recipe not found.');
          }
        }, 1000);
      });
    }

    
    function searchRecipe() {
      const recipeName = document.getElementById('recipeName').value.toLowerCase();
      const resultContainer = document.getElementById('result');
      resultContainer.innerHTML = 'Searching...';

      // Using Callback
      searchRecipeByNameCallback(recipeName, (error, result) => {
        if (error) {
          // If not found using Callback, try with Promise
          searchRecipeByNamePromise(recipeName)
            .then(result => {
              displayResult(result);
            })
            .catch(error => {
              resultContainer.innerHTML = error;
            });
        } else {
          // If found using Callback, display the result
          displayResult(result);
        }
      });
    }

    function displayResult(result) {
      const resultContainer = document.getElementById('result');
      resultContainer.innerHTML = `<h2>${result.name}</h2><p>Ingredients: ${result.ingredients.join(', ')}</p>`;
    }
  </script>
</body>
</html>
