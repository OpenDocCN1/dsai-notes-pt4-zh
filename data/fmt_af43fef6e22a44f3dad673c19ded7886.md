
Try it in AI Studio

# Advanced Editing, Restoration & Colorization

The model excels at complex edits via conversational prompting. This includes "In-painting" (removing/adding objects), "Restoration" (fixing old photos), "Colorization" (Manga/B&W photos), and "Style Swapping."

## Best Practices:

+   Semantic Instructions: You do not need to manually mask; simply tell the model what to change naturally.

+   Physics Understanding: You can ask for complex changes like "fill this glass with liquid" to test physics generation.

## Example Prompts:

> Object Removal & In-painting:

> "Remove the tourists from the background of this photo and fill the space with logical textures (cobblestones and storefronts) that match the surrounding environment."

![](img/a653da12f6ac0bd61ecdf07484051c90.png)

Try it in AI Studio

(Note: Requires uploading a photo)

> Manga/Comic Colorization:

> [Input black and white manga panel]

> "Colorize this manga panel. Use a vibrant anime style palette. Ensure the lighting effects on the energy beams are glowing neon blue and the character's outfit is consistent with their official colors."

![](img/2ae53db94409cd934a09d627412387b4.png)

Try it in AI Studio

(Note: Requires uploading an image)

> **L**ocalization (Text Translation + Cultural Adaptation):

> [Input image of a London bus stop ad]

> "Take this concept and localize it to a Tokyo setting, including translating the tagline into Japanese. Change the background to a bustling Shibuya street at night."

![](img/5370505e916083479c22e7ba4cc2f000.png)

Try it in AI Studio

(Note: Requires uploading an image)

> Lighting/Seasonal Control:

> [Input image of a house in summer]

> "Turn this scene into winter time. Keep the house architecture exactly the same, but add snow to the roof and yard, and change the lighting to a cold, overcast afternoon."

![](img/e4bb303796347dff9d54c3fd38b90e4c.png)

Try it in AI Studio

(Note: Requires uploading an image)

# Dimensional Translation (2D ↔ 3D)

A powerful new capability is translating 2D schematics into 3D visualizations, or vice versa. This is ideal for interior designers, architects, and meme creators.

## Example Prompts:

> **2D Floor Plan to 3D Interior Design Board:**

> "Based on the uploaded 2D floor plan, generate a professional interior design presentation board in a single image. Layout: A collage with one large main image at the top (wide-angle perspective of the living area), and three smaller images below (Master Bedroom, Home Office, and a 3D top-down floor plan). Style: Apply a Modern Minimalist style with warm oak wood flooring and off-white walls across ALL images. Quality: Photorealistic rendering, soft natural lighting."

![](img/28614a61fdbb37fe3892860d2b3d2fdd.png)

Try it in AI Studio

(Note: Requires uploading a floor plan)

> 2D to 3D Meme Conversion:

> "Turn the 'This is Fine' dog meme into a photorealistic 3D render. Keep the composition identical but make the dog look like a plush toy and the fire look like realistic flames."

![](img/23de9a0ff948b88a3026bf3b9f03437b.png)

Try it in AI Studio

# High-Resolution & Textures

Nano-Banana Pro supports native 1K to 4K image generation. This is particularly useful for detailed textures or large-format prints.

## Best Practices:

+   Explicitly request high resolutions (2K or 4K) if your API/Interface allows.

+   Describe high-fidelity details (imperfections, surface textures).

## Example Prompts:

> 4K Texture Generation:

> "Harness native high-fidelity output to craft a breathtaking, atmospheric environment of a mossy forest floor. Command complex lighting effects and delicate textures, ensuring every strand of moss and beam of light is rendered in pixel-perfect resolution suitable for a 4K wallpaper."

![](img/74b9312174d4413d4f5dbc74a5dcbf1f.png)

Try it in AI Studio

> Complex Logic (Thinking Mode):

> "Create a hyper-realistic infographic of a gourmet cheeseburger, deconstructed to show the texture of the toasted brioche bun, the seared crust of the patty, and the glistening melt of the cheese. Label each layer with its flavor profile."

![](img/a07823cb25448484b5b6bb1e3de7b872.png)

Try it in AI Studio

# Thinking & Reasoning

Nano-Banana Pro defaults to a "Thinking" process where it generates interim thought images (not charged) to refine composition before rendering the final output. This allows for data analysis and solving visual problems.

## Example Prompts:

> Solve Equations:

> "Solve log_{x^2+1}(x^4-1)=2 in C on a white board. Show the steps clearly."

![](img/eab9d9a53bfc4dce89a53077fce6614d.png)

Try it in AI Studio

> Visual Reasoning:

> "Analyze this image of a room and generate a 'before' image that shows what the room might have looked like during construction, showing the framing and unfinished drywall."

![](img/4ffcae66a9626b1d047673d58e54d72f.png)

Try it in AI Studio

(Note: Requires uploading an image)

# One-Shot Storyboarding & Concept Art

You can generate sequential art or storyboards without a grid, ensuring a cohesive narrative flow in a single session. This is also popular for "Movie Concept Art" (e.g., fake leaks of upcoming films).

## Example Prompt:

> "Create an addictively intriguing 9-part story with 9 images featuring a woman and man in an award-winning luxury luggage commercial. The story should have emotional highs and lows, ending on an elegant shot of the woman with the logo. The identity of the woman and man and their attire must stay consistent throughout but they can and should be seen from different angles and distances. Please generate images one at a time. Make sure every image is in a 16:9 landscape format."

Try it in AI Studio

# Structural Control & Layout Guidance

Input images aren't limited to character references or subjects to edit. You can use them to strictly control the composition and layout of the final output. This is a game-changer for designers who need to turn a napkin sketch, a wireframe, or a specific grid layout into a polished asset.

## Best Practices:

+   Drafts & Sketches: Upload a hand-drawn sketch to define exactly where the text and object should sit.

+   **Wireframes:** Use screenshots of existing layouts or wireframes to generate high-fidelity UI mockups.

+   **Grids:** Use grid images to force the model to generate assets for tile-based games or LED displays.

## Example Prompts:

> Sketch to Final Ad:

> "Create a ad for a [product] following this sketch."

Try it in AI Studio

(Note: Requires uploading a sketch)

> UI Mockup from Wireframe:

> "Create a mock-up for a [product] following these guidelines."

![](img/8057286849f51948dc4c90e06f211711.png)

Try it in AI Studio

(Note: Requires uploading a wireframe)

> Pixel Art & LED Displays:

> "Generate a pixel art sprite of a unicorn that fits perfectly into this 64x64 grid image. Use high contrast colors."

> (Tip: Developers can then programmatically extract the center color of each cell to drive a connected 64x64 LED matrix display).

![](img/17fd8cdfa1208fd1e6c6004172ae51bb.png)

Try it in AI Studio

(Note: Requires uploading a grid image)

> **Sprites:**

> "Sprite sheet of a woman doing a backflip on a drone, 3x3 grid, sequence, frame by frame animation, square aspect ratio. Follow the structure of the attached reference image exactly."

> (Tip: You can then extract each cell and make a gif)

![](img/5b31c0e980a5679f709acae109035bc1.png)

https://x.com/i/status/1994480371061469306

[Try it in Colab](https://colab.sandbox.google.com/github/google-gemini/cookbook/blob/main/quickstarts/Get_Started_Nano_Banana.ipynb#scrollTo=xuQeyK-teUf1)

# What's Next?

Now that you have mastered the basics of prompting, here is how you can start building:

+   Experiment in the UI: Google AI Studio is the fastest way to test prompts and parameters.

+   Check really cool Nano-banana powered app in the [App Gallery](https://aistudio.google.com/apps?source=showcase&showcaseTag=nano-banana).
