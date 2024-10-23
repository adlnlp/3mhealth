# [3M-Health: Multimodal Multi-Teacher Knowledge Distillation for Mental Health Detection](https://doi.org/10.48550/arXiv.2407.09020)

### <div align="center">Rina Carines Cabral, Siwen Luo, Josiah Poon, Soyeon Caren Han<br><br>Accepted at the 33rd ACM International Conference</br>on Information and Knowledge Management</br>(CIKM 2024)<br> [paper](https://doi.org/10.1145/3627673.3679635) | [supplementary](https://github.com/adlnlp/3mhealth/blob/main/CIKM_3M-Health_Supplementary.pdf)</div>

**Abstract:** The significance of mental health classification is paramount in contemporary society, where digital platforms serve as crucial sources for monitoring individuals' well-being. However, existing social media mental health datasets primarily consist of text-only samples, potentially limiting the efficacy of models trained on such data. Recognising that humans utilise cross-modal information to comprehend complex situations or issues, we present a novel approach to address the limitations of current methodologies. In this work, we introduce a **M**ultimodal and **M**ulti-Teacher Knowledge Distillation model for **M**ental **Health** Classification, leveraging insights from cross-modal human understanding. Unlike conventional approaches that often rely on simple concatenation to integrate diverse features, our model addresses the challenge of appropriately representing inputs of varying natures (e.g., texts and sounds). To mitigate the computational complexity associated with integrating all features into a single model, we employ a multimodal and multi-teacher architecture. By distributing the learning process across multiple teachers, each specialising in a particular feature extraction aspect, we enhance the overall mental health classification performance. Through experimental validation, we demonstrate the efficacy of our model in achieving improved performance.

---

Required packages:
```
pip install librosa
pip install git+https://github.com/suno-ai/bark.git
```

Datasets:
- TwitSuicide [[paper](https://arxiv.org/abs/2206.08673)]
- DEPTWEET [[paper](https://doi.org/10.1016/j.chb.2022.107503)] [[source](https://github.com/mohsinulkabir14/DEPTWEET)]
- IdenDep [[paper](https://doi.org/10.18653/v1/W18-5903)] [[source](https://github.com/Inusette/Identifying-depression)]
- SDCNL [[paper](https://doi.org/10.1007/978-3-030-86383-8_35)] [[source](https://github.com/ayaanzhaque/SDCNL)]
  
Downloadable Model Files (optional):
- [MM-EMOG embeddings](https://drive.google.com/file/d/1jSC0c2MrWpr9uk8no_2wYnvALipb7L_0/view?usp=sharing) - multi-emotion wordpiece embeddings \[[paper](https://doi.org/10.3390/robotics13030053)\] used for initializing the emotion teacher model
- Finetuned Teacher Models - models expected from Step 2 and 3
    - [TwitSuicide](https://drive.google.com/file/d/1uyZMYIY0WURxTG79DC4cSL9I7Pi9G9n9/view?usp=sharing), [DEPTWEET](https://drive.google.com/file/d/1X4wbqGv1vrrnts7qZs74GYxW6IgNB7Hv/view?usp=sharing), [IdenDep](https://drive.google.com/file/d/16_NnEksfdem0C8VxxTmVGQiumxo1g4RP/view?usp=sharing), [SDCNL](https://drive.google.com/file/d/1owmEj7UCAqAlC7_Po8PudyMhNqVJ86vX/view?usp=sharing)

## Methodology
### Step 1: Generate Audio
File: _"bark_audio_conversion.ipynb"_

Generates audio files from text input. Due to data privacy, we are unable to release the exact audio files used in our study. Use this notebook to convert the text data to audio files.

1. Required package: [Bark](https://github.com/suno-ai/)
2. Add text data
```
texts = np.array()
```
	
Expected outputs:
- _"\_AUDIO/Sample\_{id}.wav"_ - wav file generated from text

### Step 2: Finetune Text and Emotion Teacher Models
File: _"finetune_text_emo_teachers.ipynb"_

Finetuning of [BERT](https://huggingface.co/google-bert/bert-base-uncased), [RoBERTa](https://huggingface.co/FacebookAI/roberta-base), [MentalBERT](https://huggingface.co/mental/mental-bert-base-uncased), [ClinicalBERT](https://huggingface.co/medicalai/ClinicalBERT) and [MM-EMOG](https://github.com/adlnlp/mm_emog) (Emotion Teacher) 

NOTE: [Huggingface login](https://huggingface.co/docs/huggingface_hub/en/quick-start#login-command) is required to train MentalBERT

1. Set the following variables:
```
emoEmbPath = ""		#MM-EMOG path (_MODELS/MMEMOG)
datasetName = ""	#Dataset identifier for saving model
```
2. Add data
```
raw_sentences = np.array()	#Text for finetuning
raw_labels = np.array()		#Labels for finetuning
```
	
Expected outputs:
- _"\_MODELS/{datasetName}\_{model}.pt"_ - finetuned teacher models 
	
### Step 3: Finetune Audio Teacher
File: _"finetune_audio_teacher.ipynb"_

1. Required package: librosa
2. Set variables
```
datasetName = ""	#Dataset identifier for saving model
AUDIO_PATH = "" 	#Folder where audio files are contained	
raw_labels = ""		#Labels for finetuning
```
	
Expected outputs:
- "\_MODELS/{datasetName}\_MIT_ast-finetuned-audioset-10-10-0.4593.pt" - finetuned AST model

### Step 4: Train Student
File: _"train\_student\_model.ipynb"_

Use finetuned teacher models from Step 2 and 3 or download the provided model files to distill knowledge to the student model.

1. Required package: librosa
2. Set variables
```
teacher_checkpoints = []	#List of teachers to use; just have at least 1
model_paths = {}		#Dictionary of teacher model paths 
session_title = ""		#Title for file saving
```
2. Add data
```
original_train_sentences = []
original_val_sentences = []
original_test_sentences = []

original_train_labels = []
original_val_labels = []
original_test_labels = []

#NOTE: Load audio using librosa
# librosa.load(audioPath, sr = 16000) #AST required sample rate
original_train_audio = []
original_val_audio = []
original_test_audio = []
```

Expected outputs:
- _"\_OUTPUT/{session_title}\_results.csv"_ - performance of each run
- _"\_OUTPUT/{session_title}\_bestOf{#bestRun}.csv"_ - predictions for the best run
- _"\_OUTPUT/{session_title}\_cm.png"_ - confusion matrix for the best run

## Citation 
_To be updated to CIKM citation once officially published_
```
@inproceedings{10.1145/3627673.3679635,
author = {Cabral, Rina Carines and Luo, Siwen and Poon, Josiah and Han, Soyeon Caren},
	title = {3M-Health: Multimodal Multi-Teacher Knowledge Distillation for Mental Health Detection},
	year = {2024},
	isbn = {9798400704369},
	publisher = {Association for Computing Machinery},
	address = {New York, NY, USA},
	url = {https://doi.org/10.1145/3627673.3679635},
	doi = {10.1145/3627673.3679635},
	abstract = {The significance of mental health classification is paramount in contemporary society, where digital platforms serve as crucial sources for monitoring individuals' well-being. However, existing social media mental health datasets primarily consist of text-only samples, potentially limiting the efficacy of models trained on such data. Recognising that humans utilise cross-modal information to comprehend complex situations or issues, we present a novel approach to address the limitations of current methodologies. In this work, we introduce a Multimodal and Multi-Teacher Knowledge Distillation model for Mental Health Classification, leveraging insights from cross-modal human understanding. Unlike conventional approaches that often rely on simple concatenation to integrate diverse features, our model addresses the challenge of appropriately representing inputs of varying natures (e.g., texts and sounds). To mitigate the computational complexity associated with integrating all features into a single model, we employ a multimodal and multi-teacher architecture. By distributing the learning process across multiple teachers, each specialising in a particular feature extraction aspect, we enhance the overall mental health classification performance. Through experimental validation, we demonstrate the efficacy of our model in achieving improved performance.},
	booktitle = {Proceedings of the 33rd ACM International Conference on Information and Knowledge Management},
	pages = {152–162},
	numpages = {11},
	keywords = {knowledge distillation, mental health classification, multimodal},
	location = {Boise, ID, USA},
	series = {CIKM '24}
}
```

