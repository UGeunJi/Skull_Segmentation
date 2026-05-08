# Skull_Segmentation


## Purpose

```
두경부 결손 환자 재건 수술을 위한 부품 설계 보조 AI
```


## Process


#### Skull Segmentation &#10145; Skull Implant


1. DB 준비 (NC)
2. pre-processing
   - Image Enhancement
3. Segmentation
   - Develop
     - pre-training

4. 결손 Subject 생성 - [생성 코드](https://github.com/Jianningli/SciData)
5. Imaplant Learning
   - Develop 


<br>
   
--- 

<br>


### DB
#### Open DB
- CQ500 (51명)
- IMPACT (60명)
- Totalsegmentor v2 (63명)

#### Private DB
- 삼성병원 (109명)


### Model

```
U-Net / nnU-Net Version2
```

### Pre-processing

| 항목 | U-Net | orient / resampling | nnUNet |
| --- | --- | --- | --- |
| HU clip | [-1000, 3000] | 동일 | 0.5/99.5 percentile (자동) |
| Norm | /4000 &#10145; uint8 | 동일 | Z-score (자동) |
| Resampling | X | 사용자 지정 | median spacing (자동) + anisotropy 처리 |
| Orientation | X | LPS/RAS | X |
| Foreground crop | X | X | V |
| Patch size | 고정 | 고정 | 자동 결정 (GPU 최적) |
| Augmentation | 사용자 정의 | 동일 | 풍부한 default set |
| 5-fold CV | 별도 구현 | 별도 구현 | 자동 |

<br>

### 전처리 / 모델 성능 비교 Table



