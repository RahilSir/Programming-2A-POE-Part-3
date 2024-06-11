# Programming-2A-POE-Part-3

<Window x:Class="RecipeApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="Recipe App" Height="700" Width="800">
    <Grid>
        <StackPanel Margin="10">
            <!-- Existing controls -->
            <TextBlock Text="Recipe Name:" FontWeight="Bold" Margin="0,0,0,10"/>
            <TextBox Name="RecipeNameTextBox" Width="300"/>

            <TextBlock Text="Ingredients:" FontWeight="Bold"/>
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="Name:" VerticalAlignment="Center" Width="50"/>
                <TextBox Name="IngredientNameTextBox" Width="100"/>
                <TextBlock Text="Quantity:" VerticalAlignment="Center" Width="60" Margin="10,0,0,0"/>
                <TextBox Name="IngredientQuantityTextBox" Width="50"/>
                <TextBlock Text="Unit:" VerticalAlignment="Center" Width="40" Margin="10,0,0,0"/>
                <TextBox Name="IngredientUnitTextBox" Width="50"/>
                <TextBlock Text="Calories:" VerticalAlignment="Center" Width="60" Margin="10,0,0,0"/>
                <TextBox Name="IngredientCaloriesTextBox" Width="50"/>
                <TextBlock Text="Food Group:" VerticalAlignment="Center" Width="80" Margin="10,0,0,0"/>
                <TextBox Name="IngredientFoodGroupTextBox" Width="100"/>
                <Button Content="Add Ingredient" Click="AddIngredient_Click" Margin="10,0,0,0"/>
            </StackPanel>

            <ListBox Name="IngredientsListBox" Margin="0,10,0,10"/>

            <TextBlock Text="Steps:" FontWeight="Bold"/>
            <StackPanel Orientation="Horizontal">
                <TextBox Name="StepDescriptionTextBox" Width="500"/>
                <Button Content="Add Step" Click="AddStep_Click" Margin="10,0,0,0"/>
            </StackPanel>

            <ListBox Name="StepsListBox" Margin="0,10,0,10"/>

            <Button Content="Save Recipe" Click="SaveRecipe_Click" Margin="0,10,0,0"/>

            <TextBlock Text="Recipes:" FontWeight="Bold" Margin="10,0,0,0"/>
            <ListBox Name="RecipesListBox" SelectionChanged="RecipesListBox_SelectionChanged" Margin="0,10,0,10"/>

            <TextBlock Name="TotalCaloriesTextBlock" Text="Total Calories: " FontWeight="Bold" Margin="10,0,0,0"/>

            <!-- New filter controls -->
            <TextBlock Text="Filter Recipes:" FontWeight="Bold" Margin="10,0,0,0"/>
            <StackPanel Orientation="Horizontal" Margin="0,10,0,10">
                <TextBlock Text="Ingredient Name:" VerticalAlignment="Center" Width="120"/>
                <TextBox Name="FilterIngredientNameTextBox" Width="100"/>
                <TextBlock Text="Food Group:" VerticalAlignment="Center" Width="80" Margin="10,0,0,0"/>
                <TextBox Name="FilterFoodGroupTextBox" Width="100"/>
                <TextBlock Text="Max Calories:" VerticalAlignment="Center" Width="80" Margin="10,0,0,0"/>
                <TextBox Name="FilterMaxCaloriesTextBox" Width="100"/>
                <Button Content="Apply Filters" Click="ApplyFilters_Click" Margin="10,0,0,0"/>
            </StackPanel>
        </StackPanel>
    </Grid>
</Window>









using System;
using System.Collections.Generic;
using System.Linq;
using System.Windows;

namespace RecipeApp
{
    public partial class MainWindow : Window
    {
        private List<Recipe> recipes = new List<Recipe>();
        private Recipe currentRecipe = new Recipe();

        public MainWindow()
        {
            InitializeComponent();
        }

        private void AddIngredient_Click(object sender, RoutedEventArgs e)
        {
            string name = IngredientNameTextBox.Text;
            if (double.TryParse(IngredientQuantityTextBox.Text, out double quantity))
            {
                string unit = IngredientUnitTextBox.Text;
                if (int.TryParse(IngredientCaloriesTextBox.Text, out int calories))
                {
                    string foodGroup = IngredientFoodGroupTextBox.Text;

                    currentRecipe.Ingredients.Add(new Ingredient
                    {
                        Name = name,
                        Quantity = quantity,
                        Unit = unit,
                        Calories = calories,
                        FoodGroup = foodGroup
                    });

                    IngredientsListBox.Items.Add($"{quantity} {unit} of {name} - {calories} cal ({foodGroup})");

                    IngredientNameTextBox.Clear();
                    IngredientQuantityTextBox.Clear();
                    IngredientUnitTextBox.Clear();
                    IngredientCaloriesTextBox.Clear();
                    IngredientFoodGroupTextBox.Clear();
                }
                else
                {
                    MessageBox.Show("Invalid calories. Please enter a valid number.");
                }
            }
            else
            {
                MessageBox.Show("Invalid quantity. Please enter a valid number.");
            }
        }

        private void AddStep_Click(object sender, RoutedEventArgs e)
        {
            string description = StepDescriptionTextBox.Text;
            currentRecipe.Steps.Add(new Step { Description = description });
            StepsListBox.Items.Add($"{currentRecipe.Steps.Count}. {description}");

            StepDescriptionTextBox.Clear();
        }

        private void SaveRecipe_Click(object sender, RoutedEventArgs e)
        {
            string recipeName = RecipeNameTextBox.Text;
            if (string.IsNullOrWhiteSpace(recipeName))
            {
                MessageBox.Show("Recipe name cannot be empty.");
                return;
            }

            currentRecipe.Name = recipeName;
            recipes.Add(currentRecipe);
            recipes = recipes.OrderBy(r => r.Name).ToList();

            RecipesListBox.Items.Clear();
            foreach (var recipe in recipes)
            {
                RecipesListBox.Items.Add(recipe.Name);
            }

            currentRecipe = new Recipe();
            RecipeNameTextBox.Clear();
            IngredientsListBox.Items.Clear();
            StepsListBox.Items.Clear();
        }

        private void RecipesListBox_SelectionChanged(object sender, System.Windows.Controls.SelectionChangedEventArgs e)
        {
            if (RecipesListBox.SelectedItem != null)
            {
                string selectedRecipeName = RecipesListBox.SelectedItem.ToString();
                var selectedRecipe = recipes.FirstOrDefault(r => r.Name == selectedRecipeName);

                if (selectedRecipe != null)
                {
                    currentRecipe = selectedRecipe;
                    DisplayRecipe(selectedRecipe);
                }
            }
        }

        private void DisplayRecipe(Recipe recipe)
        {
            IngredientsListBox.Items.Clear();
            StepsListBox.Items.Clear();

            foreach (var ingredient in recipe.Ingredients)
            {
                IngredientsListBox.Items.Add($"{ingredient.Quantity} {ingredient.Unit} of {ingredient.Name} - {ingredient.Calories} cal ({ingredient.FoodGroup})");
            }

            for (int i = 0; i < recipe.Steps.Count; i++)
            {
                StepsListBox.Items.Add($"{i + 1}. {recipe.Steps[i].Description}");
            }

            int totalCalories = recipe.TotalCalories();
            TotalCaloriesTextBlock.Text = $"Total Calories: {totalCalories}";
            if (totalCalories > 300)
            {
                MessageBox.Show("Total calories exceed 300!", "Warning", MessageBoxButton.OK, MessageBoxImage.Warning);
            }
        }

        private void ApplyFilters_Click(object sender, RoutedEventArgs e)
        {
            string ingredientName = FilterIngredientNameTextBox.Text.ToLower();
            string foodGroup = FilterFoodGroupTextBox.Text.ToLower();
            bool maxCaloriesValid = int.TryParse(FilterMaxCaloriesTextBox.Text, out int maxCalories);

            var filteredRecipes = recipes.Where(r =>
            {
                bool ingredientMatch = string.IsNullOrEmpty(ingredientName) ||
                                       r.Ingredients.Any(i => i.Name.ToLower().Contains(ingredientName));
                bool foodGroupMatch = string.IsNullOrEmpty(foodGroup) ||
                                      r.Ingredients.Any(i => i.FoodGroup.ToLower().Contains(foodGroup));
                bool caloriesMatch = !maxCaloriesValid || r.TotalCalories() <= maxCalories;

                return ingredientMatch && foodGroupMatch && caloriesMatch;
            }).OrderBy(r => r.Name).ToList();

            RecipesListBox.Items.Clear();
            foreach (var recipe in filteredRecipes)
            {
                RecipesListBox.Items.Add(recipe.Name);
            }
        }
    }

    public class Ingredient
    {
        public string Name { get; set; }
        public double Quantity { get; set; }
        public string Unit { get; set; }
        public int Calories { get; set; }
        public string FoodGroup { get; set; }
    }

    public class Step
    {
        public string Description { get; set; }
    }

    public class Recipe
    {
        public string Name { get; set; }
        public List<Ingredient> Ingredients { get; set; }
        public List<Step> Steps { get; set; }

        public Recipe()
        {
            Ingredients = new List<Ingredient>();
            Steps = new List<Step>();
        }

        public int TotalCalories()
        {
            int total = 0;
            foreach (var ingredient in Ingredients)
            {
                total += ingredient.Calories;
            }
            return total;
        }
    }
}








