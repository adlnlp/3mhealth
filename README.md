# [3M-Health: Multimodal Multi-Teacher Knowledge Distillation for Mental Health Detection](https://doi.org/10.48550/arXiv.2407.09020)

### <div align="center">Rina Carines Cabral, Siwen Luo, Josiah Poon, Soyeon Caren Han<br><br>Accepted at the 33rd ACM International Conference on Information and Knowledge Management</br>(CIKM 2024)<br> \[[paper](https://arxiv.org/abs/2404.13645)\]|\[[supplementary](https://github.com/adlnlp/3mhealth/blob/main/CIKM_3M-Health_Supplementary.pdf)\]</div>

**Abstract:** The significance of mental health classification is paramount in contemporary society, where digital platforms serve as crucial sources for monitoring individuals' well-being. However, existing social media mental health datasets primarily consist of text-only samples, potentially limiting the efficacy of models trained on such data. Recognising that humans utilise cross-modal information to comprehend complex situations or issues, we present a novel approach to address the limitations of current methodologies. In this work, we introduce a **M**ultimodal and **M**ulti-Teacher Knowledge Distillation model for **M**ental **Health** Classification, leveraging insights from cross-modal human understanding. Unlike conventional approaches that often rely on simple concatenation to integrate diverse features, our model addresses the challenge of appropriately representing inputs of varying natures (e.g., texts and sounds). To mitigate the computational complexity associated with integrating all features into a single model, we employ a multimodal and multi-teacher architecture. By distributing the learning process across multiple teachers, each specialising in a particular feature extraction aspect, we enhance the overall mental health classification performance. Through experimental validation, we demonstrate the efficacy of our model in achieving improved performance.

---
Code and other relevant materials will be made public after the conference proceedings.



	pip install librosa
	pip install git+https://github.com/suno-ai/bark.git
	


##Generating Audio
File: "bark_audio_conversion.ipynb"

Due to data privacy, we are unable to release the exact audio files used in our study. Use this notebook to convert the text data to audio files.

1. Install Bark https://github.com/suno-ai/

2. Add text data
	texts = np.array()
	
Expected outputs:
	Audio generated from text with filename format of "_AUDIO/Sample_{id}.wav"

##Finetune Teacher Models
### Text and Emotion Teachers
File: "finetune_text_emo_teachers.ipynb"

Finetuning of BERT, RoBERTa, MentalBERT, ClinicalBERT and MM-EMOG (Emotion Teacher) https://github.com/adlnlp/mm_emog

NOTE: Huggingface login is required https://huggingface.co/docs/huggingface_hub/en/quick-start#login-command to train MentalBERT

1. Set the following variables:
	emoEmbPath = ""				#MM-EMOG path (_MODELS/MMEMOG)
	datasetName = ""			#Dataset identifier for saving model

2. Add data
	raw_sentences = np.array()	#Text for finetuning
	raw_labels = np.array()		#Labels for finetuning
	
Expected outputs:
	Finetuned models in "_MODELS" folder (default) with filename "_MODELS/{datasetName}_{checkpoint}.pt"
	
### Audio Teacher
File: "finetune_audio_teacher.ipynb"

1. Install librosa

2. Set variables
	datasetName = ""	#Dataset identifier for saving model
	AUDIO_PATH = "" 	#Folder where audio files are contained	
	raw_labels = ""		#Labels for finetuning
	
Expected outputs:
	Finetuned AST model in "_MODELS" folder (default) with filename "_MODELS/{datasetName}_{checkpoint}.pt"

##Train Student
File: "train_student_model.ipynb"

1. Set variables
	teacher_checkpoints = []	#List of teachers to use; just have at least 1
	model_paths = {}			#Dictionary of teacher model paths 
	session_title = ""			#Title for file saving

## Citation (will be updated to CIKM citation once released)
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

