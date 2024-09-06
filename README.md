## Overview: "Packs" of Photo Prompts

This script allows you to create "packs" of photo prompts. A **pack** is a set of descriptions (which we’ll call **cards**), combined with a **shared prompt section** that gives common context or style to all the cards in the pack.

### Example
If you want to create a series of film noir-style photos, you can set the **shared prompt** to something like:

`"(black and white photo:1.4), (1950s era:1.2), (cinematic:1.2), (noir:1.2)"`

For the individual **cards**, which describe each specific photo, you could have:

- `"a detective with a gun standing by a car"`
- `"a femme fatale in an office"`
- `"a suspicious old man in a trenchcoat"`

The script will combine the **shared prompt** and the **cards** to generate different variations of the images. You can also specify how many images of each type you want. This means you can set up the script, walk away, and come back to a collection of images for each description.

### How to Use

1. **Create packs** of images by defining a shared prompt and individual cards.
2. **List the packs** you want to generate images for by updating the `pack_names` array.
3. **Run the script** to generate the images for each pack.

For style inspiration, you might want to check out [this repository](https://github.com/roblaughter/style-reference), which has excellent examples of style prompts.

---

### Script Code

```javascript
// Number of images to create for each card (description)
const number_of_each_photo = 20;

// List of packs to process (you can add or remove packs from this list)
const pack_names = [
  "noir",
  "autumn landscape"
];

// Define the packs with shared prompts and specific card descriptions
const packs = {
  "noir": {
     // The shared style/prompt for all images in this pack
     shared: "(black and white photo:1.4), (1950s era:1.2), (cinematic:1.2), (noir:1.2), high contrast, deep shadows, sharp details, natural light, nostalgic, dramatic lighting, medium close-up, urban setting, reflective surfaces, 50mm lens, f/2.8, atmospheric, historic, evocative",
     
     // The specific descriptions (cards) for each image in this pack
     cards: [
        "a woman standing outside a 1950s diner at night",
        "a young smart man outside an office building",
        "an old working man in a 1950s garage",
        "a male detective holding a revolver",
        "an old woman outside a movie theatre"
      ]
   },
   
   // Another pack, focusing on autumn landscapes
   "autumn landscape": {
      shared: "(vibrant autumn landscape photo:1.4), (dramatic lighting:1.2), (rich colors:1.2), (high contrast:1.2), dynamic sky, intricate details, natural light, wide-angle, cascading waterfalls, vibrant foliage, soft shadows, 24mm lens, f/5.6, atmospheric, serene, evocative, majestic, textured, scenic",
      cards: [
        "a large oak tree in the sunshine",
        "a waterfall at dawn",
        "seaside cliffs",
        "a long shot of gentle hills"
      ]
   }
};

// Create an empty list to store the selected packs
const selected_packs = [];
for (pack of pack_names) {
  // Add the packs from 'pack_names' to 'selected_packs'
  selected_packs.push(packs[pack])
}

// Combine shared prompts and cards into full prompts
const prompts = []
for (let pack of selected_packs) {
  // Loop through each card in the pack
  for (let card of pack.cards) {
    // Combine the card description with the shared prompt
    prompt = `${card} ${pack.shared}`;
    // Add the full prompt to the 'prompts' list
    prompts.push(prompt);
  }
}

const configuration = pipeline.configuration; // Get the configuration for the image generation pipeline

// Loop to create the specified number of images for each prompt
for (let batch = 0; batch < number_of_each_photo; batch++) {
  for (let prompt of prompts) {
    canvas.clear();  // Clear the canvas before each new image
    configuration.seed = -1;  // Use a random seed for each image to get variations
    
    // Generate the image based on the current prompt and configuration
    pipeline.run({
      configuration: configuration,
      prompt: prompt
    });
  }  
}

console.log("Job complete. Open Console to see job report.");
```

---

### Key Points
- **number_of_each_photo**: This controls how many times each card description will be used to generate images.
- **pack_names**: This is where you list the packs you want to process. Only packs in this list will be used by the script.
- **packs object**: This is where you define both the **shared prompt** and the **cards** (specific image descriptions) for each pack.
- The script will **combine** the shared prompt and card for each image, allowing you to generate multiple variations based on the same base idea.
