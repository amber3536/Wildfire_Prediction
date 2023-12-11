# Wildfire_Prediction

Wildfire is a major environmental problem around the world.
Wildfire prediction is necessary for early detection of fires, to control burn areas and raise community preparedness.
Our project is to build a generative adversarial network, which would allow conditioning image generation on additional geospatial features for accurate prediction of the spread of wildfire. We use cDCGAN and UNet cGAN FT models.

## cDCGAN
Our solution, a conditional deep convolutional generative adversarial network (cDCGAN), provides freedom and accuracy in prediction.
In the generator, the prior noise input is concatenated with conditional images after convolution to form a joint hidden representation in order to predict next day fire mask. 

## UNet cGAN FT
Our UNet cGAN FT includes a generator and a discriminator, both of which use ResUNet as a backbone. A generator uses ResUNet, pretrained on a next day fire prediction objective. A discriminator, on the other hand, uses a randomly initialized ResUNet encoder. The fine-tuning objective is similar to the GAN, except that the generator has deterministic output as it is not conditioned on noise.

## Our data
<img width="1196" alt="Screen Shot 2023-12-03 at 10 20 52 AM" src="https://github.com/amber3536/Wildfire_Prediction/assets/36279682/a8c20f0c-3b4b-499b-898f-565d5c2f580f">


Our data consists of 10 features (today’s fire plus other atmospheric and geographic data) and a label, tomorrow’s fire. All features and label are in 64x64 chips which are a regional bounding box approximately 20 sq. miles. We analyzed 21.550 fires from the years 2012-2021.
Above is an example of a fire in Florida from April 2012 (the feature minimum temperature was omitted for space).
Data is from FIRMS VIIRS satellite, ERA5, ESA Worldcover 2020, and Copernicus DEM public databases

## Results
<img width="555" alt="Screen Shot 2023-12-09 at 4 18 21 PM" src="https://github.com/amber3536/Wildfire_Prediction/assets/36279682/71a972ba-df94-4eef-a304-becd2d51d527">

The main evaluation shows that the proposed cDCGAN and ResUNet cDCGAN slightly outperform the baselines in the F1 score, and their predictions exhibit less similarity to the ground truth labels as well as previous day fire masks
The main baselines Resunet and Satunet show nearly similar metrics: Satunet has mean F1 score of 0.98847, which is slightly bigger than Resunet F1 score 0.99036.
However, the simplest strategy of copying previous fire region is surprisingly effective and results in mean F1 score 0.98357.
Large values of all metrics can be explained by a highly imbalanced dataset, biased towards non-fire regions, which are trivial to predict.
Good performance of naive copy baseline can be partially explained by the same reason, in addition to the natural similarity between fire regions.
The proposed cDCGAN and ResUNet cDCGAN underperform in SSIM metric compared to the baselines: although it suggests both models might still need improvements, low SSIM scores might also be explained by the competitive nature of the GAN training.
A generator would need to produce dissimilar values since discriminator could easily detect predictions resembling previous day mask. 
The fact that the Resunet and Satunet baselines barely outperform the naive copy baselines suggests that the impact of new features might be limited.
In order to understand the impact of new features, we conducted an ablation study and trained the models without the previous day mask, possibly the strongest predictor of the next day fire mask.



## Conclusion
For our project, we implemented models predicting wildfire in a 500x500 meter region using the previous day's wildfire as well as other atmospheric and geographic data as features. We implemented a cDCGAN model, a UNet cGAN FT model, as well as baselines SatUNet and ResUNet. We found that our models had strong F1 scores, but less strong SSIM scores relative to the baselines. The explanation we suggest is that our proposed GAN-trained models tend to rely less on the fire mask copy anti-pattern, while the baseline solutions trained on image-to-image translation task tend to resort to the fire mask copy in many cases. We conclude that cDCGAN and UNet cGAN FT models have potential in the field of wildfire prediction, and await further exploration of this topic.
