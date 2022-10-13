# The First Meeting with IDL project mentors (Oct 08)

## Main steps for the project

- Understanding the problem
  - The concept of speech enhancement
  - The process of data generation
  - Explanation or interpretation for the loss function and evaluation metric 
- Benchmark and source new data
- Train models
  - Model architecture is already given 
  - Still we need to train the model 
- Tricks like using auxiliary Loss
  - We may be happy if we beat the baseline in all evaluation metric, but it will not happen.
  - Thus we should take advantage of some tricks 
- Analysis
  - We will analyze the evaluation metrics estimating perceptual quality and intelligibility. 
-  Writing and Publishing
- (Optional) Deploying and demo work
- (Optional) Participating in competition such as AWS startup competition

## How to generate data

- Although the TAs will provide the dataset, we should know that how those data are generated
- In the specific branch of [the github repository of microsoft DNS challenge](https://github.com/microsoft/DNS-Challenge/tree/interspeech2020/master/datasets), there is datasets folder which contain clean files and noise files. 
- Synthesize signal and noise to get synthetic data
  - Indeed, this synthetic data is not derived in the same way as the real-world recording in noisy environment.

## How to get a baseline to evaluate our model

- We should pass the synthetic data to Teams platform by recording it to cloud.
  - Input device : Virtual Mic
    - The reason that virtual mic is exploited is that playing the recorded file is not equivalent to the actual recording. 
    > [Comment by M.S. Kwak] The reason that a virtual mic is exploited is that we don't want other noise is involved in the transmission process. If we play the synthetic speech through a physical mic, other noise will be transmitted as well. We cannot perfectly control this kind of real noise. That's why we synthesize noisy speech and use virtual mic.
  - Platform : Microsoft Teams
  - Output device : Cloud
- This process has dependency on the hardware and internet network.
    - For the reproducibility, We should control this by using the same computers and the same wireless networks.

## Two models
- [Demucs](https://arxiv.org/pdf/2006.12847.pdf) : takes waveform as an input
- [FullSubnet](https://arxiv.org/pdf/2010.15508.pdf) : takes time-frequency spectogram as an input

## Evaluation metrics 
- TAs gave us [the list of eval metrics](https://docs.google.com/presentation/d/19cYq_MWlYc2O1fbjLo9qzem8uDkopHXLbG_JRcWSIfk/edit#slide=id.g16167ed1524_0_0)  
- STOI and PESQ are used typically.

## Tricks 
- Different loss or evaluation metrics can be utilized to help account for the things on which are not focused much by the typical papers.
  - Auxiliary loss such as the acoustic parameter loss 
  - Using acoustic features like loudness, pitch, or spectral tilt as the evaluation metrics
    - It helps evaluating perceptual quality and intelligibility
    - Probably explained in [eGeMAPS Paper](https://sail.usc.edu/publications/files/eyben-preprinttaffc-2015.pdf)

## Real-time calculation
- For real-time noise suppression, the model should not be too large.
- As setting `context` in HW1P1, we should consider about window size and stride length while training.
  - For example, setting window size as three seconds right before the given time point, and those time points are  selected discretely (obviously not continuously) by setting stride. 

## Literature Review
- Those 4 papers mentioned in Joseph's mail are sufficient
- In addition, Joseph's ICASSP paper which is not published yet may be included.

## Understanding our task
- Data can be viewed as $x = y+n$ where 
  - $y$ : signal
  - $n$ : noise
  - $x$ : synthesized one
- By passing through the chain of input device, platform, and output device, we can get $y^*$ which is the baseline for the evaluation
- We will get $\hat y$ which is the output from the similar chain where the platform(Microsoft Teams) is replaced by our model. 
> [Comment by M.S. Kwak] When "Zoom to mobile device team" asked where our model is deployed in the transmission process, Joseph answered it will be deployed between the platform and output device. But maybe this is because we are focusing on competition between cellular networks. (It might be different for "Microsoft Teams to Cloud Team")
- Evaluation can be done by measuring a kind of distance between $y$ and $\hat y$, which can be denoted as $d(y, \hat y)$
  - The baseline performance that we want to beat will be $d(y, y^*)$

## Unanswered Question
- Microsoft Teams to Cloud VS Microsoft Teams to Mobile device
  - Given that both of those task should fulfill the real-time noise suppression, what is the difference between those two? 
> [Comment by M.S. Kwak] I also had similar concern and asked it to Konan, and I think (cannot recall perfectly tho) he said hyperparameters would be different depending on which platform and device we use.

## Goal for Proposal
- Abstract
- Introduction: problem statement, model introduction
- Literature Review: 4 papers + $\alpha$
- Dataset: explain process of clean speech (in DNS) + noise
  - Demucs has a good description that we can use
- Evaluation Metrics: focus on STOI and PESQ the most, add 1 or 2 more
- Baseline Selection: research into model used by MS Teams