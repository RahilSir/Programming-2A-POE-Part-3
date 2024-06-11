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







