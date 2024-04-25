 # CustomBars - By Ray




### Overview 
CustomBars is a collection customizable bars that includes seven unique variations for integration into your Unity project. Designed to streamline the process of implementing UI elements such as health bars, mana bars, and more, this package provides a convenient solution with 20 pre-built prefabs. Each variation comes with a range of customizable elements, including colors, sprites, texts, and icons, some includes a second bar that's also customizable, allowing you to tailor the UI to your specific needs. CustomBars simplifies your game development process and enhance your user interface.




## Package Contents
![image](https://github.com/RayKelp/UnityCustomBars/assets/67763292/31b93c19-23e8-427f-9cfc-b00ff408556b)


The package contents are organized within the "Ray's > CustomBars" directory structure as follows:

- **Documentation:** Contains the documentation for the CustomBars package.
- **Prefabs:** Includes all 20 variations of CustomBars prefabs.
- **Scripts:** Houses the scripts utilized in the project, including a "Demo" folder containing scripts utilized in the demo scene controls.
- **Sprites:** Holds all the images used in the CustomBars, organized into respective folders.

Within the "CustomBars" directory, there is a demo scene showcasing all the CustomBars variations along with interactive controls for manipulating the bars.  





## Installation  
To import an Asset Store package using the Package Manager window, follow these steps:  
![image](https://github.com/RayKelp/UnityCustomBars/assets/67763292/cb52b7e6-965e-4f3b-9a22-b8e00841f6e4)

1. **Open Package Manager:**
   - Open the Package Manager window in Unity.

2. **Navigate to "My Assets":**
   - Click on the "Packages" menu in the Package Manager window.
   - Select "My Assets" from the dropdown menu.

3. **Find and Select Package:**
   - Switch the context to "My Assets".
   - Browse through the list to find the CustomBars package.
   - You can search for a specific package by name or version number.

4. **Download the Package:**
   - If the package hasn't been downloaded to your computer before, click the "Download" button in the package details view.
   - A progress bar will appear indicating the download status.

5. **Import the Package:**
   - Once the download is complete, the "Download" button will be replaced with an "Import" button.
   - Click the "Import" button to import the selected Asset Store package.

6. **Adjust Import Settings (if necessary):**
   - A "Import Unity Package" window will appear, displaying all the items ready to install.
   - Review the items selected for import and clear any items you don't want to import - We recommend to import **all**, since i'ts a very lightweight package.
   - Click "Import" to proceed.

7. **Access Imported Contents:**
   - The Package Manager will put the contents of the imported Asset Store package into the "Assets" folder of your project.
   - You can access the imported assets from your project window.

Note: You can download and import multiple packages simultaneously by using the multiple select feature.

More on how to import an Asset Store package from [Unity's official Docs](https://docs.unity3d.com/Manual/upm-ui-import.html#:~:text=Importing%20a%20complete%20project%20from%20the%20Asset%20Store,-A%20complete%20project&text=Choose%20Import%20if%20you're,to%20import%20and%20click%20Next.)

## Requirements
To utilize the CustomBars asset effectively, ensure that you meet the following basic requirements:

1. **Unity Installation:**
   - Unity Engine must be installed on your system.

2. **Unity Version 2022.3.13 or above:**
   - CustomBars was developed and tested on Unity version 2022.3.13.
   - It is recommended to use Unity 2022.3.13 or later versions for optimal compatibility.
   - If you encounter any issues with different versions, please contact us at [RaysAssets@outlook.com](mailto:RaysAssets+CustomBars@outlook.com).

3. **Gameobject with Script:**
   - To implement CustomBars in your project, you need a GameObject with a script capable of interacting with CustomBar functions.
   - This script should provide the necessary information for the CustomBar to update accordingly.
  
     
## Workflows
There are two ways of incorporating CustomBars into your project:

1. **Referencing the CustomBar into your script, then calling it's functions**
```C#
using UnityEngine;
using CustomBars; // Don't forget to add this namespace
                  // or you won't be able to reference the CustomBars classes.
                  // Or instead, you can reference the namespace before the class on each line you need
                  // (Ex: "public CustomBars.CustomBar_A" or "GetComponent<CustomBars.CustomBar_A")   

public class Implementation_Test : MonoBehaviour
{
    // Public reference
    public CustomBar_E customBar_E;

    
    float maxHealth = 200;
    float currentHealth;

    float maxMana = 100;
    float currentMana;
    

    void Start()
    {
        // Primary Bar
        currentHealth = maxHealth;

        customBar_E.SetMaxValue(maxHealth);
        customBar_E.SetCurrentValue(currentHealth);

        // Secondary Bar
        currentMana = maxMana;

        customBar_E.SetMaxValue2(maxMana);
        customBar_E.SetCurrentValue2(currentMana);
    }

    // Get damage and subtract the currentHealth
    void TakeDamage(float damage)
    {
        currentHealth -= damage;
        customBar_E.SetCurrentValue(currentHealth);
    }

    // subtract mana points
    void SpendMana(float value)
    {
        currentMana -= value;
        customBar_E.SetCurrentValue2(currentMana);
    }
}
```

2. **Using Action Events:**
   ```C#
   public class Implementation_Test2 : MonoBehaviour
   {
    // Action Events are able to send data like ints, floats, strings, bools
    public event Action<float> OnSetMaxHP;
    public event Action<float> OnSetCurrentHP;

    public event Action<float> OnSetMaxMana;
    public event Action<float> OnSetCurrentMana;


    // Using "public static event Action" eliminates the need for references,
    // but may result in unintended consequences when multiple agents affect the same bar.
    // In such cases, it's preferable to use explicit references to maintain control over each agent's interaction.
    public CustomBar_E customBar_E;

    float maxHealth = 200;
    float currentHealth;

    float maxMana = 100;
    float currentMana;
    

    void Start()
    {
        // Primary Bar
        currentHealth = maxHealth;

        OnSetMaxHP?.Invoke(maxHealth); // The "?." only triggers the event if it's being listened somewhere
        OnSetCurrentHP?.Invoke(currentHealth);

        // Secondary Bar
        currentMana = maxMana;

        OnSetMaxMana?.Invoke(maxMana);
        OnSetCurrentMana?.Invoke(currentMana);
    }

    // Get damage and subtract the currentHealth
    void TakeDamage(float damage)
    {
        currentHealth -= damage;
        customBar_E.SetCurrentValue(currentHealth);
    }

    // subtract mana points
    void SpendMana(float value)
    {
        currentMana -= value;
        customBar_E.SetCurrentValue2(currentMana);
    }
    
   }
   ```
- Note: If you choose to utilize Action Events, you'll need to reference the script containing the events in the CustomBar script.
  - To achieve this, add a reference to the script containing the events in the CustomBar script.
  - Then, assign the events to the appropriate functions within the CustomBar script.
  - There's already a section for doing this, it can be found after line 133 in each script.
    ``` C#
       public class CustomBar_E : MonoBehaviour
       {...
           ////////////////////////////////
           //// MonoBehavior functions ////
           ////////////////////////////////
           
           public Implementation_Example2 implementation_Example2; // Script containing the action events
    
           void OnEnable() // Subscribing to events
           {
               if (implementation_Example2 != null)
               {
                   implementation_Example2.OnSetMaxHP += SetMaxValue;
                   implementation_Example2.OnSetCurrentHP += SetCurrentValue;
   
                   implementation_Example2.OnSetMaxMana += SetMaxValue2;
                   implementation_Example2.OnSetCurrentMana += SetCurrentValue;
               }
           }
           void OnDisable() // Unsubscribe to avoid unwanted behaviors
           {
               if (implementation_Example2 != null)
               {
                   implementation_Example2.OnSetMaxHP -= SetMaxValue;
                   implementation_Example2.OnSetCurrentHP -= SetCurrentValue;
   
                   implementation_Example2.OnSetMaxMana -= SetMaxValue2;
                   implementation_Example2.OnSetCurrentMana -= SetCurrentValue;
               }
           }
       ...
       }

    ```
   - Note 2: When using Action Events that pass parameters such as floats, ints, or strings, ensure that the functions assigned to these events also accept the corresponding parameters (floats, ints, strings).






## Advanced
#### CustomBar Functionality

The functionality of the CustomBar is organized into four main for clarity and ease of understanding.

##### MonoBehaviour Functions

The MonoBehaviour functions include `OnEnable()`, `OnDisable()`, `OnValidate()`, `Start()`, and `Update()`. The OnValidate() is responsible for showing changes in real time on the Editor.

##### Bar Updating Functions

The bar updating functions are responsible for dynamically updating the appearance and behavior of the CustomBar.

- `UpdateBar()`: Gradually transitions the bar filling to match the current value.
- `RefreshBar()`: Updates the bar's appearance and behavior, pausing the update after 2 seconds of inativity.
- `CheckForChanges()`: Is not used in the final version, but can be enabled in case you want the bar to update by detecting if it's values were changed, instead of by being triggered by the functions.

##### Initialization Functions

These functions handle the initialization of the CustomBar component, setting up its appearance and starting values.

- `BuildTextIndicator()`: Initializes the text indicator displayed in the middle of the bar.
- `BuildBarAppearance()`: Customizes the appearance of the bar based on specified preferences.

##### Interaction Functions

The interaction functions allow for dynamic control over the CustomBar's values and appearance.
Some of them includes:
- `SetMaxValue(float value)`: Sets the maximum value of the bar.
- `Decrease(float value)`: Subtracts the specified value from the current value of the bar.
- `Increase(float value)`: Adds the specified value to the current value of the bar.
- `SetCurrentValue(float value)`: Sets the current value of the bar directly.
- `SetBarText(string text)`: Sets the text shown in the middle of the bar


## Reference
Most of the customizations can be done within the Inspector.
### CustomBar A:
![image](https://github.com/RayKelp/UnityCustomBars/assets/67763292/8ef12081-734a-45f6-b58a-a5193199ab89)

### CustomBar B:
![image](https://github.com/RayKelp/UnityCustomBars/assets/67763292/84b27443-0bac-48b1-85e9-58294fa3e440)

### CustomBars C:
![image](https://github.com/RayKelp/UnityCustomBars/assets/67763292/94e03be3-cd16-4cdf-b068-ffbd3f5bde01)

### CustomBars D:
Except for the level counter, the CustomBars D have the same properties as CustomBars C.
![image](https://github.com/RayKelp/UnityCustomBars/assets/67763292/0bccf2e4-854b-4b26-b9c8-1aff408f8bb3)

### CustomBars E:
![image](https://github.com/RayKelp/UnityCustomBars/assets/67763292/2e33fd9f-f2a0-4d13-ac33-e29fcaac82eb)


## DIY
All the visual elements can be customized, feel free to take the sprites and edit them the way you want.
It is recommended to make a copy of the sprite you want to edit, then replace on the game object "Image" component.
After that, you can edit the PNG file in a editing software of your preference.
![image](https://github.com/RayKelp/UnityCustomBars/assets/67763292/79419316-4be4-43d7-9a78-a20c0244194a)


Edit as you wish, respecting only the sprite's base format and the grey scale colors (if you choose to color the sprite inside of the editing software, you won't be able to adjust the color in the Unity's `Image` component within the Inspector. You can change the resolution or the total canvas size, but this will require you to adjust the size and position within the Unity editor later.
![image](https://github.com/RayKelp/UnityCustomBars/assets/67763292/183ed1b3-0e2b-4ba1-acea-71fc50712e0e)


Save the image as `png` and head back to Unity, select the gameobject which you want to edit, then replace the sprite withing the `Image` component.
![image](https://github.com/RayKelp/UnityCustomBars/assets/67763292/b87f032a-600e-4666-a8ee-eec6396abe06)

Adjust the color of the image, and the gameobject's scale and position if needed.
![image](https://github.com/RayKelp/UnityCustomBars/assets/67763292/6174ca28-af70-4dc4-869c-50f8d842dd45)

New elements can also be added to the CustomBars by creating new gameobjects within the `Hierarchy`, adding an `Image` component, creating a new `Sprite (2D and UI)` within the `Project`, then dragging it to the new gameobject's Image component's sprite container. 
1. **Create GameObject**: Right-click in the Hierarchy window and select "Create Empty" or duplicate an existing one within the CustomBar.

2. **Add Image Component**: Select the new GameObject, click "Add Component" in the Inspector window, search for "Image," and add the Image component - skip if you duplicated a gameobject that already has an Image component.

3. **Create Sprite**: Create a new `Sprite (2D and UI)` in the Project window, then edit it as you wish.

4. **Assign Sprite**: Drag the Sprite from the Project window onto the Image component's "Sprite" field in the Inspector window.

5. **Adjust Position and Size**: Adjust the position and size of the GameObject using the Scene view, and the color of the `Image` component within the Inspector.
![image](https://github.com/RayKelp/UnityCustomBars/assets/67763292/aa3a171b-8bc2-44fb-b58a-018ad8a2a300)





## Tutorials
