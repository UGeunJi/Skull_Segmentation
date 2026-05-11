# Skull_Segmentation


## - Purpose

```
두경부 결손 환자 재건 수술을 위한 부품 설계 보조 AI
```


## - Process


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


### - DB

<br>

#### Open DB
- CQ500 (51명)
- IMPACT (60명)
- Totalsegmentor v2 (63명)

<br>

#### Private DB
- 삼성병원 (109명)

<br>

### - Model

```
U-Net / nnU-Net Version2
```

<br>

### - Pre-processing Table

| 항목 | U-Net | orient / resampling | nnUNet |
| :---: | :--: | :--: | :--: |
| HU clip | [-1000, 3000] | 동일 | 0.5/99.5 percentile (자동) |
| Norm | /4000 &#10145; uint8 | 동일 | Z-score (자동) |
| Resampling | X | 사용자 지정 | median spacing (자동) + anisotropy 처리 |
| Orientation | X | LPS/RAS | X |
| Foreground crop | X | X | V |
| Patch size | 고정 | 고정 | 자동 결정 (GPU 최적) |
| Augmentation | 사용자 정의 | 동일 | 풍부한 default set |
| 5-fold CV | 별도 구현 | 별도 구현 | 자동 |

<br>

### - Preprocessing / Model Performance Comparison Table

<table border="1" style="border-collapse: collapse; width: 100%; table-layout: fixed;">
    <thead style="background-color: #f2f2f2;">
        <tr>
            <th colspan="2" rowspan="2" style="padding: 10px; text-align: center; vertical-align: middle;">Dataset</th>
            <th rowspan="2" style="text-align: center; vertical-align: middle;">Subjects</th>
            <th rowspan="2" style="text-align: center; vertical-align: middle;">(Orient / Resample)<br>(2D &rarr; 3D)</th>
            <th colspan="2" style="text-align: center; vertical-align: middle;">Model</th>
            <th rowspan="2" style="text-align: center; vertical-align: middle;">Patchwork</th>
        </tr>
        <tr>
            <th style="text-align: center; vertical-align: middle;">U-Net</th>
            <th style="text-align: center; vertical-align: middle;">nnU-Net</th>
        </tr>
    </thead>
    <tbody style="text-align: center; vertical-align: middle;">
        <tr>
            <td rowspan="8" style="font-weight: bold; text-align: center; vertical-align: middle;">Experiments</td>
            <td rowspan="4" align="center" style="text-align: center; vertical-align: middle;">삼성서울</td>
            <td rowspan="2" align="center" style="text-align: center; vertical-align: middle;">13</td>
            <td align="center" style="height: 35px; text-align: center; vertical-align: middle;">-</td> 
            <td align="center" style="text-align: center; vertical-align: middle;">0.9468</td>
            <td align="center" style="text-align: center; vertical-align: middle;"><strong>0.9665</strong></td>
            <td align="center" style="text-align: center; vertical-align: middle;">-</td>
        </tr>
        <tr>
            <td align="center" style="text-align: center; vertical-align: middle;">V</td>
            <td align="center" style="text-align: center; vertical-align: middle;">0.8837</td>
            <td align="center" style="text-align: center; vertical-align: middle;"></td>
            <td align="center" style="text-align: center; vertical-align: middle;">-</td>
        </tr>
        <tr>
            <td align="center" rowspan="2" style="text-align: center; vertical-align: middle;">109</td>
            <td align="center" style="height: 35px; text-align: center; vertical-align: middle;">-</td>
            <td align="center" style="text-align: center; vertical-align: middle;">0.7859</td>
            <td align="center" style="text-align: center; vertical-align: middle;">0.7878</td>
            <td align="center" style="text-align: center; vertical-align: middle;">-</td>
        </tr>
        <tr>
            <td align="center" style="text-align: center; vertical-align: middle;">V</td>
            <td align="center" style="text-align: center; vertical-align: middle;">0.8835</td>
            <td align="center" style="text-align: center; vertical-align: middle;"><strong>0.9423</strong></td>
            <td align="center" style="text-align: center; vertical-align: middle;">-</td>
        </tr>
        <tr>
            <td align="center" rowspan="2" style="text-align: center; vertical-align: middle;">CQ500</td>
            <td align="center" rowspan="2" style="text-align: center; vertical-align: middle;">51</td>
            <td align="center" style="height: 35px; text-align: center; vertical-align: middle;">-</td>
            <td align="center" style="text-align: center; vertical-align: middle;">0.9533</td>
            <td align="center" style="text-align: center; vertical-align: middle;"><strong>0.9735</strong></td>
            <td align="center" style="text-align: center; vertical-align: middle;">-</td>
        </tr>
        <tr>
            <td align="center" style="text-align: center; vertical-align: middle;">V</td>
            <td align="center" style="text-align: center; vertical-align: middle;">0.9536</td>
            <td align="center" style="text-align: center; vertical-align: middle;"></td>
            <td align="center" style="text-align: center; vertical-align: middle;">-</td>
        </tr>
        <tr>
            <td align="center" rowspan="2" style="text-align: center; vertical-align: middle;">IMPACT</td>
            <td align="center" rowspan="2" style="text-align: center; vertical-align: middle;">60</td>
            <td align="center" style="height: 35px; text-align: center; vertical-align: middle;">-</td>
            <td align="center" style="text-align: center; vertical-align: middle;">0.9683</td>
            <td align="center" style="text-align: center; vertical-align: middle;"><strong>0.9782</strong></td>
            <td align="center" style="text-align: center; vertical-align: middle;">-</td>
        </tr>
        <tr>
            <td align="center" style="text-align: center; vertical-align: middle;">V</td>
            <td align="center" style="text-align: center; vertical-align: middle;">0.9018</td>
            <td align="center" style="text-align: center; vertical-align: middle;"></td>
            <td align="center" style="text-align: center; vertical-align: middle;">-</td>
        </tr>
        <tr style="background-color: #f9f9f9;">
            <td align="center" rowspan="2" style="font-weight: bold; text-align: center; vertical-align: middle;">Papers</td>
            <td align="center" style="text-align: center; vertical-align: middle;">Dot et al. (2022)</td>
            <td align="center" style="text-align: center; vertical-align: middle;">453</td>
            <td align="center" style="text-align: center; vertical-align: middle;">-</td>
            <td align="center" style="text-align: center; vertical-align: middle;">-</td>
            <td align="center" style="text-align: center; vertical-align: middle;">0.9622</td>
            <td align="center" style="text-align: center; vertical-align: middle;">-</td>
        </tr>
        <tr style="background-color: #f9f9f9;">
            <td align="center" style="text-align: center; vertical-align: middle;">Steybe et al. (2022)</td>
            <td align="center" style="text-align: center; vertical-align: middle;">20</td>
            <td align="center" style="text-align: center; vertical-align: middle;">별도 전처리 적용</td>
            <td align="center" style="text-align: center; vertical-align: middle;">-</td>
            <td align="center" style="text-align: center; vertical-align: middle;">-</td>
            <td align="center" style="text-align: center; vertical-align: middle;">0.94</td>
        </tr>
    </tbody>
</table>




