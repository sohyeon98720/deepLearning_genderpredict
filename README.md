# 손 x-ray로 성별 예측하기(딥러닝 이용)

### 인공지능과 딥러닝 스터디 - 미래연구소(2019.03~2019.05)

---------------------------------

**주제** : 손 x-ray로 성별 예측하기

**기획 배경** : 의학적으로 골간 간격이나 석회화 정도로 성별판단이 가능하지만 의사들도 쉽지 않다는 점에서 딥러닝을 이용하여 빠르고 정확한 예측을 하기위해 시작하게 됨.

--------------------------------

**프로젝트 내용** :
1. 데이터 준비 : kaggle에서 *RSNA Bone Age* 데이터셋을 이용함.
    - 데이터의 수 : 사진 약 12000여장
    - 손 x-ray 사진 예시와 csv 예시

![손 x-ray 사진 예시](https://user-images.githubusercontent.com/47767202/76082540-fece2780-5fee-11ea-9cad-dcb289e0d3ac.jpg)
    
![figure2](https://user-images.githubusercontent.com/47767202/76082802-8156e700-5fef-11ea-9497-0c7e8d49e5be.PNG)
<br><br><br>

2. 데이터 확인 :
    - csv를 보고 데이터의 특성이 될 수 있는 *boneage*와 *male* 중 ***male***(True/False)을 선택함
    - data(male)가 balance를 이루므로(=female의 수와 male의 수가 비슷) accuracy를 metrics로 결정
<br><br><br>


3. 모델 설계 : 
    - train,val,test set으로 나눔
    
    - `version1. 모델 구성`
    
    ![figure3](https://user-images.githubusercontent.com/47767202/76083747-9df41e80-5ff1-11ea-96fa-b92f919bec3d.png)
      -> epoch=20, optimizer=adam                            
      -> train acc=0.6886 / val acc=0.6379 / test acc=0.6552       
      _=> 데이터의 수가 적다고 판단하여 data augmentaion을 적용 + epoch또한 늘리기로 결정_
      <br><br><br>
      
    - `version2. 모델 구성`                            
       version1에서 augmentation을 적용 + epoch=70으로 늘림                      
      -> train acc=0.7448 / val acc=0.7561 / test acc=0.8125
      ![figure4](https://user-images.githubusercontent.com/47767202/76084475-4e165700-5ff3-11ea-932a-f756c1a59a8f.png)                
      _=> 더 나은 정확도를 위해 epoch을 늘리기로 결정_
<br><br><br>

    - `version3. 모델 구성`                                                     
       epoch=120 으로 늘린 것 외에 달라진 점 X                              
       ![figure5](https://user-images.githubusercontent.com/47767202/76084893-40150600-5ff4-11ea-84c4-b5f4f323ba7e.png)                
       파란색 - train acc / 빨간색 - val acc                          
       -> epoch 70이 넘어가면서 부터 overfitting이 발생함                                       
       _=> early stopping 추가_                                                 
       _=> 최종 test acc = 70.94_


-------------------------------------------------------------

**문제점** : 최종 코드인 version3 - early stopping을 한 정확도가 version 2의 정확도 보다 낮았음

**결론** : human level error - 현직 의사들의 acc가 약 88% 부근이고 코드 상 최고 acc가 약 80%인 점으로 볼 때<br>
모델에 약간의 수정을 더하면 human lever error를 따라잡을 수 있겠다는 생각을 했다.<br>
위의 문제 해결 방법으로는 더 큰 네트워크를 만들거나 여러 하이퍼파라미터를 조정하는 방법이 있다.

------------------------------

- kaggle에서 모든 코드를 작성하였고, keras를 사용함.                        
- 결론의 human level error의 출처는 NCBI PubMed.gov - El Morsi DA, AI Hawary AA, 2013 Jan
