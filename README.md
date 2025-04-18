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






