# README #

For my **Master's Degree Thesis** I've developed an artificial intelligence system that is able to detect **arrhythmia episodes** in 15-leads ECG signals.   
   
The problems of detecting arrhythmias in ECG has been addressed as **Deep Semi-Supervised Anomaly Detection Task**. So, in this context, arrhythmias can be seen as anomalies in normal ECGs.   
   
The system's core is an ad-hoc designed Deep Convolutional AutoEncoder called **DPNet**. DPNet is not open-suorce (yet?).           

## Main Idea ##

The idea behind this system is the following: **you can't recreate what you don't recognise**.    
   
This simple but very effective idea is made concrete by exploiting the autoencoder's ability to reconstruct the input in the following way. First, the autoencoder is trained exclusively using normal ECGs, thus it will be able to learn their salient characteristics and recreate them accurately. This result in the autoencoder producing a low reconstruction error for the normal samples.    

Dually, during the evaluation phase, when to the autoencoder will be also given in input ECGs containing arrhythmias, it will not be able to recreate them properly since it has only learned the salient characteristics of the normal samples from which the anomaly ones (i.e. containing arrhythmias) differ a lot. Anomaly samples' reconstruction error is therefore sensibly higher than the normal samples' one.   

This is what happens graphically:   

**1° Lead Normal Sample Reconstrucion.**    
![](./imgs/normal_rec.png)     
       
**1° Lead Atrial Fibrillation Sample Reconstrucion.**   
![](./imgs/arr_rec.png)    
   
So, the **reconstruction error** can be used as an **anomaly indicator**.   
   
This is the idea behind an Autoencoder-based anomaly detection [algorithm](http://dm.snu.ac.kr/static/docs/TR/SNUDM-TR-2015-03.pdf).   
![](./imgs/alg.png)   


## Features ##

The proposed system differs from the others for a number of reasons:
    
* It's a **General Purpose** system while competitors are Special Purpose.      

* It supports **15-leads ECGs** instead competiors are limited to 12-leads ones.   

* It can process **Arbitrary ECGs** without any specific segmentation algorithm (e.g. R-R interval segmentation) like competitors do.


### Greatest Strength ###

The provided system **drastically reduces the human component** required to build this kind of systems.   

Competitors require completely labeled data (i.e. a label for each segment that can be extracted from an ECG) and these labels have to be assigned by human specialists (so, cardiologists). Note that this is a very expensive operation both in economic and time terms.  
   
**Competitor Labelling.**     
![](./imgs/total_labeling.png)   
    
The proposed system takes the things to the opposite direction: everything the system needs is a big enough quantity of normal samples. So, we go from the competitors' approach in which it's required a label for each segment that can be extract from an ECG, to a new approach in which only a single label for an entire normal ECG is required (no label is required for potentially anomaly samples). Once trained, the proposed system will be able to autonomously detect the ECG segments containing arrhythmias, so the burden passes from the human component to the artificial intelligence, reducing both costs and time for realizing this kind of systems. 
    
**Provided System Labelling.**     
![](./imgs/dpnet_labeling.png)   


## Evaluation ##

DPNet has been evaluated in term of ROC Curve and AUC Score. It has obtained an **AUC Score of 0.8002** which certifies both the model's quality and robustness to noise.    
