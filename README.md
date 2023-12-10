# Wildfire_Prediction

Wildfire is a major environmental problem around the world.
Wildfire prediction is necessary for early detection of fires, to control burn areas and raise community preparedness.
Our project is to build a generative adversarial network, which would allow conditioning image generation on additional geospatial features for accurate prediction of the spread of wildfire. We use cDCGAN and cGAN-FT models.

## cDCGAN
Our solution, a conditional deep convolutional generative adversarial network (cDCGAN), provides freedom and accuracy in prediction.
In the generator, the prior noise input is concatenated with conditional images after convolution to form a joint hidden representation in order to predict next day fire mask. 

## C-GAN FT
UNet C-GAN FT includes a generator and a discriminator, both of which use ResUNet as a backbone. A generator uses ResUNet, pretrained on a next day fire prediction objective. A discriminator, on the other hand, uses a randomly initialized ResUNet encoder. The fine-tuning objective is similar to the GAN, except that the generator has deterministic output as it is not conditioned on noise.

## Our data
<img width="1196" alt="Screen Shot 2023-12-03 at 10 20 52 AM" src="https://github.com/amber3536/Wildfire_Prediction/assets/36279682/a8c20f0c-3b4b-499b-898f-565d5c2f580f">


Our data consists of 10 features (today’s fire plus other atmospheric and geographic data) and a label, tomorrow’s fire. All features and label are in 64x64 chips which are a regional bounding box approximately 20 sq. miles. We analyzed 21.550 fires from the years 2012-2021.
Above is an example of a fire in Florida from April 2012 (the feature minimum temperature was omitted for space).
Data is from FIRMS VIIRS satellite, ERA5, ESA Worldcover 2020, and Copernicus DEM public databases

## Results
<img width="555" alt="Screen Shot 2023-12-09 at 4 18 21 PM" src="https://github.com/amber3536/Wildfire_Prediction/assets/36279682/71a972ba-df94-4eef-a304-becd2d51d527">

Our evaluation metrics are F1 score and the structural similarity index measure (SSIM).
We evaluate the wildfire prediction as a pixel-wise classification problem and then use F1 score with micro-averaging for the flattened 64x64 images as a primary success metric. Our models had higher F1 scores than ResUNet and SatUNet, and lower SSIM scores. Since the purpose of SSIM is image quality assessment and it only considers mean, variance, and covariance between images, we don't treat SSIM as a primary metric, but instead rely on it to interpret the predictions of the models.
For both metrics, we calculate the mean and standard deviation after running the training 3 times on different splits.

## Conclusion
For our project, we implemented models predicting wildfire in a 500x500 meter region using the previous day's wildfire as well as other atmospheric and geographic data as features. We implemented a cDCGAN model, a UNet cGAN FT model, as well as baselines SatUNet and ResUNet. We found that our models had strong F1 scores, but less strong SSIM scores relative to the baselines. We conclude that cDCGAN and UNet cGAN FT models have potential in the field of wildfire prediction, and await further exploration of this topic.
