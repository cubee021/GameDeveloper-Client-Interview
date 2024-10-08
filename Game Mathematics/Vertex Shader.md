# Vertex Shader
파이프라인의 두 번째 과정으로, 삼각형을 구성하는 각 정점의 최종 데이터를 결정하는 함수이다.

<img src="https://user-images.githubusercontent.com/42164422/107613437-ab973700-6c8b-11eb-99a8-d08c99fc0d73.png" width="600" height="300"/>

<br/>

## 공간변환 (MVP)
Model, View, Projection Matrix는 최종적으로 어떤 vertex가 화면 상의 어느 픽셀에 위치해야 하는지를 결정할 때 사용되는 행렬들이며, 이 행렬들을 서로 곱해 4x4 크기의 MVP matrix를 만들어 사용한다. 이렇게 구한 행렬을 vertex position과 곱해 화면 상의 위치를 구할 수 있다.

+ **Model Matrix**
  > 오브젝트 공간 -> 월드 공간
  
  모델 변환은 각 오브젝트의 좌표 공간을 변환하여 하나의 월드 공간에 통합하는 과정.
  
  이동(Translate), 회전(Rotate), 크기(Scale) 변환이 이루어진다.

  이 변환을 하나의 행렬로 만든 것을 TRS 행렬이라고 하는데, 벡터 V와 곱해질 때 순서는 반대이다.
  ```cpp
  S x R x T x V = S x R (T x V) = S x (R x TV) = S x RTV = SRTV
  // 따로 곱하는 경우 : VTRS
  ```

+ **View Matrix**
  > 월드 공간 -> 카메라 공간
  
  오브젝트를 화면에 그려내기 쉽도록 카메라를 기준으로 공간을 변환하는 과정.

  즉, 카메라 공간이란 카메라의 위치가 원점이고, 카메라가 보는 방향이 z축인 공간이다.

+ **Projection Matrix**
  > 카메라 공간 -> 클립 공간

  카메라 기준의 정점을 화면에 보이기 위한 정점 위치로 변환하는 과정.

  화면에 렌더링될 영역인 절두체가 정의되고, 이 경계 안에 있는 폴리곤만 유지된다. (culling & clipping)

  즉, 클립 공간이란 컬링/클리핑을 통해 최종적으로 카메라 시야에 보이는 Mesh들만 위치한 공간이다.
  
  원근감의 유무에 따른 종류 :
  + 직교투영(Orthographic Projection) ex. UI
  + 원근투영(Perspective Projection)
  
  


  
