
# Image Configuration

```json
"format": {
  "aspect_ratio": "1:1",
  "orientation": "square"
},
"environment": {
  "location": {
    "setting": "vintage indoor living room",
    "era": "1970s retro",
    "atmosphere": "warm, intimate, cinematic, calm"
  },
  "lighting": {
    "primary_source": {
      "type": "vintage floor lamp",
      "position": "left side",
      "style": "scalloped edge with fringe trim",
      "temperature": "warm tungsten",
      "behavior": "soft ambient with natural falloff"
    },
    "global_contrast": "medium"
  },
  "background_elements": {
    "walls": {
      "material": "aged matte wood paneling",
      "color": "warm brown",
      "pattern": "vertical panels"
    },
    "foliage": {
      "type": "leafy indoor plant",
      "vessel": "earthen ceramic pot",
      "placement": "background left"
    }
  }
},
"furniture_and_props": {
  "seating": {
    "type": "upholstered retro couch",
    "material": "woven patterned textile",
    "palette": ["beige", "brown", "muted green", "rust"],
    "design": "geometric pattern"
  },
  "accents": [
    {
      "item": "pillow",
      "material": "deep red velvet",
      "texture": "plush"
    },
    {
      "item": "pillow",
      "material": "off-white faux fur",
      "texture": "fluffy"
    }
  ]
},
"character_model": {
  "identity": {
    "gender": "female",
    "age": "early to mid 20s",
    "ethnicity": "light skin tone"
  },
  "anatomy_details": {
    "skin": "natural realistic texture, visible pores, subtle freckles",
    "hair": {
      "color": "dark brown",
      "style": "long, loose natural waves",
      "parting": "center"
    }
  },
  "performance": {
    "pose": "reclining on sofa, relaxed and confident, one arm on backrest",
    "expression": "soft sultry, direct gaze, slightly parted lips"
  },
  "wardrobe": {
    "item": "satin slip dress",
    "color": "deep burgundy wine red",
    "features": ["low-cut V neckline", "thin spaghetti straps", "loose draped fit"],
    "finish": "silky sheen"
  }
},
"cinematography": {
  "camera": {
    "angle": "eye-level",
    "framing": "medium portrait",
    "focus": "subject face and upper torso"
  },
  "optics": {
    "depth_of_field": "moderate",
    "background_blur": "slightly soft",
    "balance": "asymmetrical (subject right, lamp left)"
  },
  "color_profile": {
    "dominant": ["warm brown", "burgundy", "beige"],
    "accents": ["deep red", "off-white"],
    "grading": "warm cinematic, lifted blacks, soft highlights"
  }
},
"render_quality": {
  "positive_attributes": [
    "high resolution",
    "photorealistic",
    "sharp fabric textures",
    "realistic skin rendering"
  ],
  "negative_prompt": [
    "cartoon",
    "anime",
    "illustration",
    "overexposed",
    "harsh shadows",
    "blurred face",
    "distorted anatomy",
    "extra limbs"
  ]
}
}
```

![](img/13f45ce220a7021c006b1d5cd36cf90e.png)

# Project Configuration

```json
{
  "project_constraints": {
    "facial_rendering": "100% original facial features (Do not edit the face)",
    "resolution": "1200x1200px",
    "output_quality": "Photo-realistic, 8K resolution"
  },
  "camera_and_style": {
    "device_emulation": "Y2k-era digital camera (Canon IXUS / Sony Cyber-shot)",
    "perspective": "Low-angle, shot from behind at a 3/4 angle",
    "visual_aesthetic": "Cinematic, nostalgic",
    "post_processing": {
      "grain": "Thin film grain",
      "depth_of_field": "Shallow",
      "color_grading": "Bright overall tone with a slight pinkish-purple hue"
    }
  },
  "subject_details": {
    "demographics": "Beautiful young woman",
    "physique": "Hourglass figure, fair dewy skin",
    "hair": "Long, wavy, thick milk-brown hair, spiral curls reaching waist",
    "makeup": {
      "base": "Light brown nude",
      "eyes": "Aussie eyeliner",
      "finish": "Glossy cheek and lip color"
    },
    "nails": "Long, polished in shiny wine red"
  },
  "pose_and_action": {
    "position": "Sitting sideways in passenger seat",
    "hands": "Right hand gripping steering wheel, left hand lifting hair up",
    "expression": "Looking back over shoulder, seductive and confident"
  },
  "fashion_and_accessories": {
    "top": "Shiny white tube top with open back and delicate crisscross detail",
    "bottom": "Light blue high-waisted jeans (fitted)",
    "jewelry": "Several gold rings, matching bracelet",
    "bag": "Small Chanel Vanity bag with black gold chain (on armrest)"
  },
  "environment": {
    "location": "Interior of luxury SUV (Mercedes-Benz style)",
    "interior_elements": "Wooden trim, light beige leather seats",
    "time_of_day": "Night"
  },
  "lighting": {
    "technique": "Direct flash photography",
    "sources": [
      "Soft warm interior lighting",
      "Cool blue dashboard light",
      "City light bokeh through windows"
    ],
    "characteristics": "High contrast, pronounced flash shadows, realistic cinematic lighting"
  }
}
```

![](img/398014babab79b4e3cc9e43327f8ff0f.png)

# Prompt: Couple Selfie

```json
{
  "image_description": {
    "scene": "A couple taking a selfie in a brightly lit store, likely a toy or souvenir shop, filled with plush toys and merchandise. The photo is taken in a mirror.",
    "subjects": [
      {
        "person_id": 1,
        "gender": "male",
        "clothing": "Black long-sleeved sweater",
        "accessories": "Fox (Nick Wilde from Zootopia) character hat, white watch",
        "pose": "Kissing/puckering lips, arm around the female subject, looking at the camera reflection.",
        "details": "Light skin tone, short blonde hair"
      },
      {
        "person_id": 2,
        "gender": "female",
        "clothing": "Bright pink halter top, white pearl necklace",
        "accessories": "Rabbit (Judy Hopps from Zootopia) character hat, rings, yellow smartphone (iPhone) taking the selfie",
        "pose": "Smiling broadly, holding phone up for selfie, arm around male subject, looking at the camera reflection.",
        "details": "Light skin tone, long grey hair"
      }
    ],
    "background_elements": [
      "Shelves filled with various Zootopia plush toys (Nick Wilde, Judy Hopps, etc.)",
      "Bright, colorful overhead lights (pendant lights)",
      "String lights/decorations",
      "Other merchandise visible on shelves in the distance"
    ],
    "mood": "Playful, joyful, affectionate"
  }
}
```

![](img/13fa58219f345458d4d992e0cee57d9f.png)

# Prompt: Vertical Three-Panel Collage

```json
{
  "image_structure": {
    "layout": "Vertical three-panel collage",
    "aspect_ratio": "Tall vertical strip",
    "shot_type": "Close-up selfies"
  },
  "subject_details": {
    "demographics": "Young woman with long, wavy brunette hair",
    "skin": "Natural skin texture, minimal or no makeup",
    "clothing": "Pink lace-trimmed bralette or camisole top with thin spaghetti straps",
    "accessories": "Long manicured nails (nude/pale pink color)"
  },
  "environment": {
    "setting": "Dimly lit bedroom or hotel room interior",
    "background_elements": [
      "Beige/cream colored walls",
      "Heavy brown curtains on the left side",
      "Partial view of a bed headboard in the background"
    ]
  },
  "lighting_and_style": {
    "lighting": "Warm, indoor artificial lighting, low-light conditions",
    "aesthetic": "Candid selfie style, slightly grainy phone camera quality, early 2010s social media aesthetic, flash photography look",
    "mood": "Casual, intimate, playful"
  },
  "panel_breakdown": {
    "top_panel": {
      "expression": "Pouting or confused, eyebrows slightly furrowed",
      "pose": "Finger hooked over bottom lip, looking directly at camera"
    },
    "middle_panel": {
      "expression": "Soft smile, looking directly at camera",
      "overlay_effect": "Digital pink heart crown filter (Snapchat style) floating around the head"
    },
    "bottom_panel": {
      "expression": "Relaxed, mouth slightly open, gazing at camera",
      "pose": "Head tilted to the side, hand gently touching chin and lower lip"
    }
  }
}
```

![](img/7d3abfbcbe9a0799fe3d1404ec770554.png)

# Custom Description

Use the original face from the uploaded photo. A young woman taking a mirror selfie, with very long and voluminous waves dark hair and soft wispy bangs, natural glam makeup - soft pink dewy blush, glossy red pouty lips, gold geometric hoop earrings, wearing a fitted white and red ringer very cropped top with cute cartoon face of Betty Boop with the word "Betty Boop" below in cursive font, a silver waistchain and visible toned midriff, holding a smartphone with a patterned case, confident expression.

Set in a nostalgic early-2000s bedroom: pastel walls, chunky wooden dresser, CD player, posters of 2000s pop icons, hanging beaded door curtain, cluttered vanity with lip glosses and digital camera.
