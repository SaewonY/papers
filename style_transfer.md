# Neural Style Transfer 2015

<br>

두 이미지(content, style)가 주어졌을 때 그 이미지의 주된 형태는 content image와 유사하게 유지하면서
스타일만 style image와 유사하게 바꾸는 기술

<br>

![1](https://user-images.githubusercontent.com/40786348/46254531-d671fd80-c4cb-11e8-9b53-fa71f3a8a95f.PNG)

<br>
<br>

## - Introduction
<br>

+ 2015 딥러닝 분야에서 획기적인 논문이 하나 arxiv에 업로드가 되는데 그것이 바로 Neural Style Tranfer이다. 

+ 이 논문이 당시 발표되었을 시에 무엇이 달랐기에 주목을 받았을까? 기존의 cnn은 주어진 이미지에서 거듭된 layer를 통해서 의미가 있는 feature를 뽑아내고, cnn의 깊은 layer로 갈수록 더 좋은 feature를 뽑아내기 떄문에 더 좋은 성능을 낸다고 간단하게 정리를 할 수 있겠다. 

+ 하지만 당시에는 이와 반대로 feature들을 통해서 이미지를 복원하는 연구가 한창 진행중이었다. Style transfer 논문 저자들의 기여는 바로 cnn layer에 있어서 **content와 style을 재구성하는 방법**을 제안하였다는 점이다. 

+ 본 글에서는 스터디에서 다루었던 내용을 복습하는 차원에서 간략하게 기술해 본다. 

<br>
<br>

## - Method

<br>

![0](https://user-images.githubusercontent.com/40786348/46254747-eb9c5b80-c4ce-11e8-8e84-8c6951b157d8.PNG)


### Transfer Learning


+ 논문의 저자는 transfer learning으로 **vgg19 network**를 사용한다. 
  (Transfer Learning이란 다양한 과제를 통해 이미 훈련된 네트워크를 새로운 과제 학습에 가져다 사용하는 것을 뜻한다) 

+ 논문에 따르면, 저자는 모델을 16개의 conv layer와 5개의 pooling layer로 구성하였으며 fully connected layer는 전혀 사용하지 않았음을 밝히고 있다.

+ 또한 max-pooling 보다는 average-pooling을 차용하였는데, 이는 후자가 전자보다 약간의 성능이 더 나왔기 때문으로 추측할 수 있겠다. 

<br>

## Loss Function

<br>

![6](https://user-images.githubusercontent.com/40786348/46254698-2a7de180-c4ce-11e8-92e3-4e3c03467b55.PNG)

<br>

NST (Neural Style Transfer)의 손실 함수는 다음과 같이 3가지 단계로 구성되어 있다.

첫 번째로는 content image의 손실 함수를 계산하고

두 번째로는 style image의 손실 함수를 계산한 이후, 

마지막에 이 둘을 합쳐서 generated image의 손실 함수를 계산한다. (여기서 G는 generated image를 가리키는 것으로, 최종  이미지를 가리킨다)

<br>

### 1 . Content Loss

<br>

우리가 원하는 것은 generated image와 content image가 비슷하도록 하는 것이다. 

여기서 우리는 content image에서 하나의 hidden layer의 activation을 선택하고 마찬가지로 generated image에서 같은 layer의 activation을 선택한다.

(논문에 의하면 너무 얕거나 너무 깊은 레이어보다는 중간 정도의 layer를 선택하는 것이 가장 좋은 성능을 보인다고 주장한다) 

정리하면 다음과 같은 function이 나온다.

</br>

![1](https://user-images.githubusercontent.com/40786348/46254765-682f3a00-c4cf-11e8-80ea-d26bfa0c5beb.PNG)

</br>

여기서 C는 content image, G는 generated image를 가리킨다. 

content loss는 content image의 feature map과 generated image의 feature map을 각각 계산하고 이 둘 간의 차이인 Frobenius norm을 loos로 정의한다.

<br>

### 2 . Style Loss

<br>

논문에 따르면 style의 정의는 같은 layer의 서로 다른 filter 간의 correlation이라고 할 수 있다. 

Content loss가 feature map을 직접 비교한 것과는 다르게 style loss는 각 feature map에 대하여 gram matrix를 구하고,

content loss와 유사하게 이 gram matrix 간 차이인 Frobenius norm을 loss로 정의한다. 

<br>

![2](https://user-images.githubusercontent.com/40786348/46254876-4767e400-c4d1-11e8-8541-d4a6db86ad46.PNG)

(여기서 G는 Gram matrix의 G를 가리킨다)

현재까지는 하나의 layer에 대한 loss를 구한 상태이다. 

여기서 우리는 다음과 같이 다른 layer들의 weight까지 merge한다면 더 좋은 결과를 얻을 수 있을 것이다.

</br>

![12345](https://user-images.githubusercontent.com/40786348/46255260-ebec2500-c4d5-11e8-8df6-dd7de3f3dec2.PNG)

</br>

### 3 . Total Cost

</br>

마지막으로 이전에 구한 content loss와 style loss를 바탕으로 최종 cost를 계산한다.

여기서 알파와 베타는 각각 하이퍼 파라미터로 content와 style cost 간의 상대적인 가중치를 구체화시킬 수 있다.

</br>

![123](https://user-images.githubusercontent.com/40786348/46255083-f0174300-c4d3-11e8-8b6e-f3d1ca31a32f.PNG)



## Reference

- [논문 pdf](https://arxiv.org/abs/1508.06576v2)

- http://sanghyukchun.github.io/92/

- Coursera Deeplearning Course