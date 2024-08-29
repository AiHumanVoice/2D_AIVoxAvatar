# VoiceCloning과 Audio2Video를 이용한 대화형 AI 아바타 
---
## 등장배경

사용자가 선택한 목소리와 이미지로 가상의 인간을 만드는 것을 목표로 했습니다. 단순히 인간의 모습을 가진 챗봇을 넘어, 인간의 삶을 보조하는 AI 비서의 역할까지 확장하고자 했습니다. 이러한 목표를 달성하기 위해 관련 기술을 연구하고 직접 구현해보는 것이 이 프로젝트를 시작하게 된 계기입니다.

---
## 모델 소개
본 프로젝트는 크게 CV부분과 TTS부분 그리고 RAG으로 나누어서 진행되었습니다.
### CV
CV 부분은 aniportrait를 사용하여 나만의 남자(여자)친구의 외모를 생성하였습니다.

aniportrait의 main component는 다음과 같습니다.

1) Audio2Lmk
  - 오디오 파일 -> 2D facial landmark 시퀀스로 변형
  - 오디오 피쳐를 통해 입술, 머리의 움직임을 파악

2) Lmk2Video
  - 2D landmark sequence를 통해 portrait video 생성

### TTS
TTS 부분은 프롬프트를 주게 되면 가상의 목소리를 만들어주는 역할을 하였습니다.

TTS의 main component는 다음과 같습니다.

1) GPT(Generative Pre-Trained Transformer)
2) HiFi-GAN Decoder
  - MelSpectogram -> Audio
3) Tokenizer
  - Text -> Token Embedding
4) Speaker Embedding
  - 화자의 음색 복제

<img width="298" alt="스크린샷 2024-08-24 오후 2 33 53" src="https://github.com/user-attachments/assets/bf74b90f-65ac-4fb3-b262-a786bd0f078f">

### RAG
<img width="312" alt="스크린샷 2024-08-24 오후 2 34 32" src="https://github.com/user-attachments/assets/0be8bd5c-c51b-4e19-8729-f95b9bd7c7d0">

---
## DataSet
### Aniportrait
1) 단독 발화자 정면 1시간 영상
2) AI hub -- "립러닝 음성 인식 데이터"

### TTS
1) 데이터 종류: 시상식 수상소감
2) 레퍼런스 음성 종류: 전지현, 김범, 김수현, 김지원
3) 

---
## Experiment
### Aniportrait
  - 목표: 한국어 음성에 대한 Talking Face 추론 정확도 향상 -> Aniportrait의 Audio2Mesh Finetuning

### TTS
  - Finetuning Condition: reference audio(.wav)의 길이가 2분 이상이어야 함 + 외부 노이즈가 첨가되면 안됨
  - TTS Training Loss Track
    - Loader Time: 각 에폭마다 모델이 로드되는 시간을 트래킹
    - loss_text_ce: 텍스트에 초점을 맞춘 cross-entropy loss
    - loss_mel_ce: 멜스펙토그램에 초점을 맞춘 cross-entropy loss
    - total_loss: loss_text_ce + loss_mel_ce
    <img width="330" alt="스크린샷 2024-08-24 오후 2 45 26" src="https://github.com/user-attachments/assets/f327309b-72e9-4587-8ac1-5f65fe33d763">

---
## Role
- 김홍주: PM, Aniportrait Finetuning
- 김민서: RAG, XTTS
- 김재영: RAG, GPT
- 문재영: Aniportrait Finetuning, UI
- 정윤서: XTTS Fintuning, Github Management



