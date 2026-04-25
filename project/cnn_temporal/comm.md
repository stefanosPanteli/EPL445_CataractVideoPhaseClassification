# 1
## Dataset
- `window_size`: 3

## Architecture
- `pool_type`: mean
- `meta_hidden`: 16

## Stats
### Epoch 7
| **Model** | **Accuracy** | **Precision** | **Recall** | **F1** | **Weighted F1** |
| --- | --- | --- | --- | --- | --- |
| **CNN_temporal_balanced** | 0.7550 | 0.7777 | 0.7550 | 0.6793 | 0.7547 |
| **CNN_temporal_balanced_win3** | 0.7591 | 0.7813 | 0.7591 | 0.6836 | 0.7584 |
| **CNN_temporal_balanced_win5** | 0.7631 | 0.7839 | 0.7631 | 0.6872 | 0.7618 |
| **CNN_temporal_balanced_win7** | 0.7708 | 0.7896 | 0.7708 | 0.6939 | 0.7684 |
| **CNN_temporal_balanced_win15** | 0.7872 | 0.8063 | 0.7872 | 0.7119 | 0.7835 |
| **CNN_temporal_balanced_win15_15** | 0.7895 | 0.8091 | 0.7895 | 0.7135 | 0.7854 |
| **CNN_temporal_balanced_win31** | **0.8044** | 0.8237 | 0.8044 | ***0.7345*** | 0.7996 |

### Epoch 10
| **Model** | **Accuracy** | **Precision** | **Recall** | **F1** | **Weighted F1** |
| --- | --- | --- | --- | --- | --- |
| **CNN_temporal_balanced** | 0.7633 | 0.8009 | 0.7633 | 0.6789 | 0.7653 |
| **CNN_temporal_balanced_win3** | 0.7664 | 0.8044 | 0.7664 | 0.6816 | 0.7683 |
| **CNN_temporal_balanced_win5** | 0.7725 | 0.8123 | 0.7725 | 0.6885 | 0.7747 |
| **CNN_temporal_balanced_win7** | 0.7799 | 0.8201 | 0.7799 | 0.6969 | 0.7825 |
| **CNN_temporal_balanced_win15** | 0.7960 | 0.8374 | 0.7960 | 0.7178 | 0.7993 |
| **CNN_temporal_balanced_win15_15** | 0.7989 | 0.8403 | 0.7989 | 0.7214 | 0.8021 |
| **CNN_temporal_balanced_win31** | **0.8084** | 0.8501 | 0.8084 | **0.7329** | 0.8103 |

---
---

<br><br>

---
---

# 2
## Dataset

## Architecture

## Stats
### Epoch X
| **Model** | **Accuracy** | **Precision** | **Recall** | **F1** | **Weighted F1** |
| --- | --- | --- | --- | --- | --- |
| **CNN_temporal_balanced** | 
| **CNN_temporal_balanced_win3** | 
| **CNN_temporal_balanced_win5** | 
| **CNN_temporal_balanced_win7** | 
| **CNN_temporal_balanced_win15** | 
| **CNN_temporal_balanced_win15_15** | 
| **CNN_temporal_balanced_win31** | 

### Epoch Y
| **Model** | **Accuracy** | **Precision** | **Recall** | **F1** | **Weighted F1** |
| --- | --- | --- | --- | --- | --- |
| **CNN_temporal_balanced** | 
| **CNN_temporal_balanced_win3** | 
| **CNN_temporal_balanced_win5** | 
| **CNN_temporal_balanced_win7** | 
| **CNN_temporal_balanced_win15** | 
| **CNN_temporal_balanced_win15_15** | 
| **CNN_temporal_balanced_win31** | 

---
---

<br><br>

---
---


# 3
## Dataset

## Architecture

## Stats
### Epoch X
| **Model** | **Accuracy** | **Precision** | **Recall** | **F1** | **Weighted F1** |
| --- | --- | --- | --- | --- | --- |
| **CNN_temporal_balanced** | 
| **CNN_temporal_balanced_win3** | 
| **CNN_temporal_balanced_win5** | 
| **CNN_temporal_balanced_win7** | 
| **CNN_temporal_balanced_win15** | 
| **CNN_temporal_balanced_win15_15** | 
| **CNN_temporal_balanced_win31** | 

### Epoch Y
| **Model** | **Accuracy** | **Precision** | **Recall** | **F1** | **Weighted F1** |
| --- | --- | --- | --- | --- | --- |
| **CNN_temporal_balanced** | 
| **CNN_temporal_balanced_win3** | 
| **CNN_temporal_balanced_win5** | 
| **CNN_temporal_balanced_win7** | 
| **CNN_temporal_balanced_win15** | 
| **CNN_temporal_balanced_win15_15** | 
| **CNN_temporal_balanced_win31** | 


---
---

<br><br>

---
---

# Overall Best
Second model with more complex prediction head, and shorter clips.
Epoch 7 with window of 31 frames.

#### Case 18
![alt text](image-1.png)

#### Case 19
![alt text](image-2.png)

#### Case 20
![alt text](image-3.png)

#### Case 21
![alt text](image-4.png)

![Confusion Matrix of All Predictions](image.png)

| **Model** | **Accuracy** | **Precision** | **Recall** | **F1** | **Weighted F1** |
| --- | --- | --- | --- | --- | --- |
| **CNN_RNN_balanced** | 0.7760 | 0.7823 | 0. | 0.7244 | 0.7722 |
| **CNN_RNN_balanced_win3** | 0.7793 | 0.7850 | 0. | 0.7287 | 0.7753 |
| **CNN_RNN_balanced_win5** | 0.7829 | 0.7890 | 0. | 0.7325 | 0.7793 |
| **CNN_RNN_balanced_win7** | 0.7875 | 0.7936 | 0. | 0.7395 | 0.7841 |
| **CNN_RNN_balanced_win15** | 0.8043 | 0.8112 | 0. | 0.7586 | 0.8008 |
| **CNN_RNN_balanced_win15_15** | 0.8050 | 0.8121 | 0. | 0.7590 | 0.8015 |
| **CNN_RNN_balanced_win31** | **0.8154** | 0.8285 | 0. | ***0.7653*** | 0.8112 |