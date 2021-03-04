# chestX

Sure I can explain why I have 40 GB of chest x-rays on my computer...

A repository for the final project of the MAIS 202 machine learning bootcamp. Wrote a CNN that diagnoses chest x-rays, choosing from 14 common disease classes. This was a multi-label, multi-class problem. The x-rays look something like this:

<center>
<img src="https://github.com/kushasareen/chest_x-rays/blob/master/00000001_000.png" width="30%">
</center>

... and there are about a hundred thousand of them.

Here's a picture of the first iteration of the model. It takes as input an image, a bounding box that may be provided and some basic patient characteristics and outputs a probability for each of the 14 disease classes:
<center>
<img src="https://github.com/kushasareen/chest_x-rays/blob/master/model.png" width="100%">
</center>

My first iteration of the model failed. I built a version of an inception net, a CNN commonly used for medical imaging (I think Google used it for its opthalmology project a couple years ago). It acheived 93% accuracy but terrible F1 and AUC scores. This was because the dataset was unbalanced. Think of it like this, if someone made the ill-founded decision of letting me diagnose patients and I decided that there's absolutely nothing wrong with anybody, technically, I'd be right 99% of the time, but I wouldn't be a very good doctor would I? The model simply had no predictive power. Because of the size of the dataset, I also had issues with RAM where I had to resize the images to 50x50 (not so good) to fit on the 8 GB of RAM on my laptop.

I came back to this project over the summer and built modelv2 which actually works. I used pre-trained weights from Google's ImageNet and added some inception and some dense layers, also accounting for patient characteristics (age, sex etc.). To fix the RAM problem, I wrote a data generator that writes preprocessed data to a .csv and reads it while training. This allowed me to train on all of the data and resize images to only 224x224 and not worry about RAM usage. Additionally, to account for the data imbalance, in addition to re-sampling, I wrote a custom weighted loss function to ensure the model had some actual predictive power.

The model achieves and AUC of 0.769 and an F1 score of 0.412, outperforming the average clinical radiologist but still underperforming the ML research titans (See Stanford's 2017 ChestXNet paper for some interesting AI-doctor comparison). I spent some time playing around with the model with my brother (who's a medical resident), first getting him to diagnose an x-ray and then running it through the model. It was interesting to see the types of things that my brother would sometimes miss and the model would pick up on and what my model wouldn't catch that my brother would get.

I hope to build modelv3 soon. I've read about some really promising papers about attention-guided models in medical imaging with awesome results AND interpretability (which isn't common for ML algorithms).
