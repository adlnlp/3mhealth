# [3M-Health: Multimodal Multi-Teacher Knowledge Distillation for Mental Health Detection](https://doi.org/10.48550/arXiv.2407.09020)

### <div align="center">Rina Carines Cabral, Siwen Luo, Josiah Poon, Soyeon Caren Han<br><br>Accepted at the 33rd ACM International Conference</br>on Information and Knowledge Management</br>(CIKM 2024)<br> [paper](https://arxiv.org/abs/2404.13645) | [supplementary](https://github.com/adlnlp/3mhealth/blob/main/CIKM_3M-Health_Supplementary.pdf)</div>

**Abstract:** The significance of mental health classification is paramount in contemporary society, where digital platforms serve as crucial sources for monitoring individuals' well-being. However, existing social media mental health datasets primarily consist of text-only samples, potentially limiting the efficacy of models trained on such data. Recognising that humans utilise cross-modal information to comprehend complex situations or issues, we present a novel approach to address the limitations of current methodologies. In this work, we introduce a **M**ultimodal and **M**ulti-Teacher Knowledge Distillation model for **M**ental **Health** Classification, leveraging insights from cross-modal human understanding. Unlike conventional approaches that often rely on simple concatenation to integrate diverse features, our model addresses the challenge of appropriately representing inputs of varying natures (e.g., texts and sounds). To mitigate the computational complexity associated with integrating all features into a single model, we employ a multimodal and multi-teacher architecture. By distributing the learning process across multiple teachers, each specialising in a particular feature extraction aspect, we enhance the overall mental health classification performance. Through experimental validation, we demonstrate the efficacy of our model in achieving improved performance.

---

Required packages
```
pip install librosa
pip install git+https://github.com/suno-ai/bark.git
```

⚠️NOTE: Finetuned teacher model states and MM-EMOG pre-trained weights will be shared soon


## Generating Audio
File: _"bark_audio_conversion.ipynb"_

Due to data privacy, we are unable to release the exact audio files used in our study. Use this notebook to convert the text data to audio files.

1. Required package: [Bark](https://github.com/suno-ai/)
2. Add text data
```
texts = np.array()
```
	
Expected outputs:
- _"\_AUDIO/Sample\_{id}.wav"_ - wav file generated from text

## Finetune Text and Emotion Teacher Models
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
	
## Finetune Audio Teacher
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

## Train Student
File: _"train\_student\_model.ipynb"_

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
@article{cabral20243mhealth,
      title={3M-Health: Multimodal Multi-Teacher Knowledge Distillation for Mental Health Detection}, 
      author={Rina Carines Cabral and Siwen Luo and Josiah Poon and Soyeon Caren Han},
      year={2024},
      eprint={2407.09020},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={https://arxiv.org/abs/2407.09020}, 
}
```

