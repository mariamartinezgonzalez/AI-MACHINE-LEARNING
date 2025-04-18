# AI-MACHINE-LEARNING
Mobile app to generate your outfits according to your own clothes. Users upload pictures of their clothing items and the app will create outfits: tops, bottoms, sets, shoes, jackets and complements. Users might ask for "chic" or "casual" outfits.
# Main goal of the project
- Recognizes different types of clothes from images using trained AI models.
- Assigns multiple descriptive labels to each clothing item (like "bottom + skirt + patterned").
- Automatically label new images you add to your Google Drive, no retraining needed.
- Combines clothing into chic or casual outfits based on predefined fashion rules.
- Shows you a complete outfit with images.
- Lets you approve or reject the outfit interactively.
- Saves your approved outfits into a .csv file for reuse.
# DATA
We created our own dataset. The first step was to collect and organize all the clothing images to train our models and build our digital wardrobe.To do that, we uploaded all the images into Google Drive, sorting them into folders based on clothing category. Each clothing class (like tops, bottoms, shoes, etc.) had its own dedicated folder inside a main folder called clothing_dataset. Once we inserted all the fotos, we end up with 70-bottoms, 70-tops, 96-sets, 49-shoes, 51-jackets and 27-complements.

Our Google drive looks like this:
clothing_dataset/
- ├── bottom/         ← pants/skirts + patterned/(plain + light/dark)
- ├── top/            ← top + patterned/(plain + light/dark)
- ├── set/            ← 2-piece outfits
- ├── shoes/          ← heels/boots/(flats + patterned/plain)
- ├── jacket/         ← coats, blazers
- ├── complement/     ← bags

After grouping the photos by their visual characteristics (type, style, tone), we created a CSV file for each category to store the labels.
Each CSV file includes:The image filename and A label with multiple tags describing that item. Each category has a labeled CSV file:
- Bottom_labeled.csv
- Top_labeled.csv
- Set_labeled.csv
- Shoes_labeled.csv
- Jacket_labeled.csv
- complement_labeled.csv

Example: Bottom_labeled.csv

    filename      label
- b5.jpg:    bottom + pants + plain + light
- bfe12.png: bottom + skirt + patterned
- be2.jpg:   bottom + pants+ patterned
- bf7.png:   bottom + skirt +  plain + dark

These CSVs were used to train our AI models to recognize and classify each new clothing image automatically. They're also the foundation for our rule-based outfit generator.

# Code in Google Colab Training:
For our project we have used the model ResNet18, it is a type of deep learning model trained to recognize objects in pictures. Since it has already been trained on millions of general images, we don’t start from scratch, we reuse what it has learned and fine-tune it with our labeled data to make it recognize our own clothes. Each clothing class (top, bottom, shoes...) has its own model. These models are trained to look at the clothing image and predict what kind of item it is. For example, the model can learn to recognize if the item is a “top”, whether it is “plain” or “patterned”, and whether it is “light” or “dark”.

We use a technique called multi-label classification, meaning one image can have more than one label. For example, bottom + pants + plain + dark. The model doesn’t justpredict one label; it predicts several at once. Also, for training we have used the BCELoss loss function, which is used when you're predicting multiple independent labels. Each label is trained separately and gets a score between 0 and 1 using a sigmoid function. Then, the model is trained to minimize the error between its predictions and the true labels. We make sure the code was accurate and efficient we selected 15 epoch

<img width="74" alt="image" src="https://github.com/user-attachments/assets/21478038-d7bc-42dc-9a88-8be510f7e66f" />



We also added a similarity fuction to check that the code was actually understanding the differences between the same type of clothes. Are there 2 tops similar? Instead of checking lables this tool checks the actual image features. This was possible thanks to the ResNet18 that is trained to see and differenciate the different shapes and colors. The model stores this visual knowledge as a long list of numbers like fingerprints, how does it work?
- Show both images to the model.
- The model creates two fingerprints (vectors).
- The code measures how similar those fingerprints are.
- The result is a number between 0 and 1.
- 1: very similar,  0: completely different.
- This number is called cosine similarity.

<img width="343" alt="image" src="https://github.com/user-attachments/assets/288b02a5-bc2a-4814-af39-a17beaf1d434" />  <img width="383" alt="image" src="https://github.com/user-attachments/assets/fb9926c9-7366-4a62-9231-dfa8439a052b" />   <img width="371" alt="image" src="https://github.com/user-attachments/assets/536bd9b7-0002-437a-b4b9-a6356931107a" />  <img width="466" alt="image" src="https://github.com/user-attachments/assets/2b09b888-7dc5-48e9-aa2a-3c92173c3b5b" />  <img width="350" alt="image" src="https://github.com/user-attachments/assets/a6d9734c-3994-43c7-b55e-578944fcc59e" />   <img width="401" alt="image" src="https://github.com/user-attachments/assets/7af872e3-7cb9-4f7c-860a-d34f260e7646" />


# Code in Google Colab Predicting New Clothes:
When you want to add new pictures of clothes, you don’t need to retrain the models.This loads the saved model, example: bottom_classifier.pt, predicts the label for the image, and adds the result to bottom_classifier.csv. This means your system is scalable. You can keep adding new items and they will automatically be included in your wardrobe.
- Look inside the folder /clothing_dataset/bottom
- Skip any images that were already labeled (it checks the CSV file bottom_labeled.csv)
- Use the trained model to predict multiple tags for each new image: ("bottom + pants + plain + light")
-  Save the results to the CSV file (bottom_labeled.csv) so the image is ready to use in outfit generation
  
<img width="432" alt="image" src="https://github.com/user-attachments/assets/d2a4e2c9-dc8e-41c3-ad83-72c2b0c96aee" />


# Code in Google Colab Rules of combination:
We have created rules for the app to generate realistic outfits and not just random combinations. First we ask the user what type of outfit it need for today.
style = input("Choose your style (casual or chic): "). If it selects casual we also ask for a coat: coat_answer = input("Do you want a coat layer? (yes/no): "). 

Chic Outfits Rules:
- Set + Heels + Complement
    - If the item is labeled as a set, it must be paired with heels and a complement (bag).
    - No other clothing is added (no top or bottom).
- Skirt + Top + Heels + Complement
    - If the item is a bottom (pants/skirt), it is combined with a top.
    - The top and bottom cannot both be patterned.
    - Heels and a complement are added.

Casual Outfits Rules:
- Top + Bottom + Shoes + Complement
    - Combines a top and bottom with either flats or sneakers.
    - Never includes heels.
    - Allows patterned clothing only if not both top and bottom are patterned.
- Optional: Jacket or Coat
    - A blazer is added to casual outfits depending on the outfit.
    - A coat is only added if the user answered "yes" to the cold-weather prompt.
    - A patterned coat is only allowed if the rest of the outfit is plain.

# Code in Google Colab Outfit Generator:
Libraries:
- pandas: saves and loads outfit combinations.
- matplotlib.pyplot + matplotlib.image: show clothing images.
- ipywidgets: buttons (Yes / No).
- IPython.display: updates output in real time
  
How does it work?
- Decides which function to call based on the user’s style choice (chic/casual)
- Once outfit is generated, 2 buttons appear:
    - Yes: you like the outfit and you will save it.
    - No: you don´t like th eoutfit, the code will generate a new one.
- The outfit is saved to the appropriate CSV file.
  
CHIC:

<img width="797" alt="image" src="https://github.com/user-attachments/assets/3c23a0a7-3b37-40cd-810b-3e4a6ca89ebf" />
<img width="785" alt="image" src="https://github.com/user-attachments/assets/26a5e151-b4ef-4284-b03e-0bf7adec9621" />
<img width="767" alt="image" src="https://github.com/user-attachments/assets/06e01527-a7f0-4f4e-b684-c635285c7bea" />


CASUAL:

<img width="907" alt="image" src="https://github.com/user-attachments/assets/cf88cca3-dfd3-46f0-8283-531f90b568fe" />
<img width="894" alt="image" src="https://github.com/user-attachments/assets/faaea166-9415-4abf-a483-1a3a4ef75ee7" />
<img width="886" alt="image" src="https://github.com/user-attachments/assets/a4c09dc8-4450-4ee1-ae6f-bfc35609c15c" />





