# SIIM ISIC Melanoma Competition - 2020
This repo contains my solution for the ISIC Melanoma(2020) competition hosted on Kaggle which secured a top 8% finish in the competition rankings.

Below is an overview of my pipeline for the competition:

## Data used
Along with this year's competition data, I used the data from last three years ISIC Melanoma competitions.

## TFRecord conversion
All the image data was converted into TFRecord format to leverage faster read speeds and reduce the overall model train time.

## Cross Validation Strategy
A triple stratified cross validation strategy was used and thanks to Chris Deotte for providing the dataset(this saved loads of time). In the validation folds only the current competition data was used. The CV strategy is explained [here](https://www.kaggle.com/c/siim-isic-melanoma-classification/discussion/165526) 

## Preprocessing/Augmentations
1. Rotation
2. Shear
3. Zoom
4. Horizontal and vertical shifts
5. Random Saturation
6. Random Brightness
7. Random Contrast
8. Random horizontal and/or vertical flips

## Models used
Inspired by [this](https://www.kaggle.com/c/siim-isic-melanoma-classification/discussion/160147) wonderful discussion post, I decided to train CNNs on variable image sizes to extract patterns of different sizes from the images. The following models were trained on each of the image sizes:
1. **Image Size**-256, **CNN Backbone**-EfficientNetB3 to EfficientNetB7
2. **Image Size**-384, **CNN Backbone**-EfficientNetB3 to EfficientNetB7
3. **Image Size**-512, **CNN Backbone**-EfficientNetB3 to EfficientNetB7
4. **Image Size**-768, **CNN Backbone**-EfficientNetB3 to EfficientNetB5 (Due to computational intensity and lack of compute resources could not train EfficientNetB6 and B7)

At the end of the CNN architecture a GlobalAveragePooling layer and a final dense layer with sigmoid activation was used for making predictions.

## Test Time Augmentation
Around 25 rounds of TTA was used to make predictions.

## Final Ensemble
A weighted ensemble of the above models was used for final submission. Weights were choosen based on best OOF CV score.
