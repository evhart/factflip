# 🖼️ FactFlip
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

> FactFlip is an image macro meme generator designed for creating fact-checking memes based on misinforming claims and their corrections.


## 📐 Archtecture
FactFlip, like [MemeCraft](https://arxiv.org/abs/2403.14652), uses a large language model to understand and generate memes. However, FactFlip focuses on creating specific memes to debunk misinformation, using humor and well-matched meme formats to make the corrections clear and engaging. 

![FactFlip Architecture](.//images/factflip_architecture.png)

## 🤖 Prompts 
The following prompts are currently used by FactFlip.

### Meme Concept Extraction (Step A)
The following prompt is used for extracting the main concept ubased on the meme template usage description.
```json
[
   {
      "role":"system",
      "content":"You are a fact-checker and meme expert that provides short and acurate answers."
   },
   {
      "role":"user",
      "content":"Just answer in one or two words without punctuation, what is the main rethorical device used in the following meme description?"
   },
   {
      "role":"user",
      "content":"<IMAGE_TEMPALTE_USAGE>"
   }
]
```

### Meme Image Captioning (Step B)
The image description is extracted using the following prompt.
```json
[
   {
      "role":"system",
      "content":"You are a fact-checker and meme expert who perfectly describes images concisely."
   },
   {
      "role":"user",
      "content":"Provide a short description for the image. Consider that the image is an image used for  <CONCEPT> and humor.",
      "images":"<IMAGE>"
   }
]
```

### Meme Claims Generation(Step C)
The claim behind a given meme content is extracted with this prompt.
```json
[
   {
      "role":"system",
      "content":"You are a fact-checker and meme expert that provides short and acurate answers."
   },
   {
      "role":"user",
      "content":"Give a claim explaining the meaning of the following meme text. The text of the meme contains <CONCEPT> and humour."
   },
   {
      "role":"user",
      "content":"The image used by the meme is <IMAGE_TITLE>. <IMAGE_TEMPLATE_DESCRIPTION> <IMAGE_TEMPALTE_USAGE>"
   },
   {
      "role":"user",
      "content":"The text of the meme is: <CAPTION>"
   },
   {
      "role":"assistant",
      "content":"The one sentence claim explaning the meme is: \""
   }
]
```

The misinforming claim is extracted as follow.
```json
[
   {
      "role":"system",
      "content":"You are a fact-checker and meme expert that provides short and acurate answers."
   },
   {
      "role":"user",
      "content":"Give a claim that contradict the following meme explanation."
   },
   {
      "role":"user",
      "content":"<CLAIM_CORRECTION>"
   },
   {
      "role":"assistant",
      "content":"The one sentence contradictory claim is: \""
   }
]
```

### Meme Caption Generation (Step 3)
The following prompt is used for generatig the meme caption. The actual prompt uses 4 examples.

```json
[
   {
      "role":"system",
      "content":"You are a fact-checker and meme expert that provides short and acurate answers."
   },
   {
      "role":"user",
      "content":"Instruction: Generate a caption to turn the image into a meme. The meme must contains <EXAMPLE_CONCEPT> and humour and be formulated as response to a erronous claim based on the provided claim review."
   },
   {
      "role":"user",
      "content":"Input image: The image is <EXAMPLE_IMAGE_TITLE>. <EXAMPLE_IMAGE_TEMPLATE_DESCRIPTION> <EXAMPLE_IMAGE_TEMPALTE_USAGE>"
   },
   {
      "role":"user",
      "content":"Input erronous claim: <EXAMPLE_CLAIM>"
   },
   {
      "role":"user",
      "content":"Input claim review:<EXAMPLE_CLAIM_CORRECTION>"
   },
   {
      "role":"assistant",
      "content":"Output meme caption: <EXAMPLE_MEME_CAPTION>"
   },
   {
      "role":"user",
      "content":"Instruction: Generate a caption to turn the image into a meme. The meme must contains <CONCEPT> and humour and be formulated as response to a erronous claim based on the provided claim review."
   },
   {
      "role":"user",
      "content":"Input image: The image is <IMAGE_TITLE>. <IMAGE_TEMPLATE_DESCRIPTION> <IMAGE_TEMPALTE_USAGE>"
   },
   {
      "role":"user",
      "content":"Input erronous claim: <CLAIM>"
   },
   {
      "role":"user",
      "content":"Input claim review: <CLAIM_CORRECTION>"
   }
]
```

The resulting meme caption is plit using the following prompt:
```json
"Just answer: Split the sentence in two: <MEME_CAPTION>"
```
