# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 이재영
- 리뷰어 : 강다은


# PRT(Peer Review Template)
- [x]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 문제에서 요구하는 최종 결과물이 첨부되었는지 확인
    - 문제를 해결하는 완성된 코드란 프로젝트 루브릭 3개 중 2개, 
    퀘스트 문제 요구조건 등을 지칭
        - 해당 조건을 만족하는 코드를 캡쳐해 근거로 첨부

      <br/>

      > 문제의 요구사항에 맞게 보행자 및 근거리 차량을 감지했을 때 "Stop"을 반환하는 함수를 올바르게 구현하였다.

      <br/>

      ```
      # 정지 조건 : 사람 한 명이상
      #if any(people in class_names for people in PEOPLE_LIST):
      #    print(people)
      #    return "Stop"
      for _,people in enumerate(class_names):
          if people in PEOPLE_LIST :
              print(people)
              return "Stop"
    
      # 정지 조건 : 차량 크기 300px 이상
      for box in detections.nmsed_boxes[0][:num_detections] / ratio :
          x1, y1, x2, y2 = box
          w = x2-x1
          h = y2-y1
          print(f'width : {w} , height : {h}')
          if w>=size_limit or h>= size_limit :
              return "Stop"
      ```
      
      <br/>
    


- [x]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 해당 코드 블럭에 doc string/annotation이 달려 있는지 확인
    - 해당 코드가 무슨 기능을 하는지, 왜 그렇게 짜여진건지, 작동 메커니즘이 뭔지 기술.
    - 주석을 보고 코드 이해가 잘 되었는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.

      <br/>

      > 함수가 동작하는 순서를 주석을 통해 명시하였다. 꼼꼼한 주석을 통해 코드를 brief하게 읽더라도 어떤 기능을 수행하는지 쉽게 이해할 수 있었다.

      <br/>

      ```
      def resize_and_pad_image(image, training=True):
          # 기본 크기 설정
          min_side = 800.0
          max_side = 1333.0
          min_side_range = [640, 1024]
          stride = 128.0

          # 입력 이미지의 크기
          image_shape = tf.cast(tf.shape(image)[:2], dtype=tf.float32)

          # 훈련 중에는 랜덤하게 최소 크기 선택
          if training:
              min_side = tf.random.uniform((), min_side_range[0], min_side_range[1], dtype=tf.float32)

          # 이미지를 리사이징할 비율 계산
          ratio = min_side / tf.reduce_min(image_shape)

          # 만약 이미지의 크기가 최대 크기를 초과하면 비율을 조정
          if ratio * tf.reduce_max(image_shape) > max_side:
              ratio = max_side / tf.reduce_max(image_shape)

          # 이미지를 계산된 비율로 리사이징
          image_shape = ratio * image_shape
          image = tf.image.resize(image, tf.cast(image_shape, dtype=tf.int32))

          # 패딩을 위한 새로운 이미지 크기 계산
          padded_image_shape = tf.cast(
              tf.math.ceil(image_shape / stride) * stride, dtype=tf.int32
              )

          # 이미지를 패딩
          image = tf.image.pad_to_bounding_box(
              image, 0, 0, padded_image_shape[0], padded_image_shape[1]
              )

          return image, image_shape, ratio
      ```
      
      <br/>


      
- [x]  **3. 에러가 난 부분을 디버깅하여 문제를 “해결한 기록을 남겼거나” 
”새로운 시도 또는 추가 실험을 수행”해봤나요?**
    - 문제 원인 및 해결 과정을 잘 기록하였는지 확인
    - 문제에서 요구하는 조건에 더해 추가적으로 수행한 나만의 시도, 
    실험이 기록되어 있는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
        
      <br/>

      > 모델 학습과정에서의 loss와 accuracy 변화를 시각화하여 학습 동향을 파악하고 분석하였다.

      <br/>

      <img width="612" alt="image" src="https://github.com/youungg/Aiffel_Quest/assets/149550222/41f99f40-8d1b-4d44-9059-c4dfe1845a1e">
     
      ```
      test accuracy 는 망가졌는데 생각보다 물체인식은 잘함
      학습할수록 loss가 떨어지는데 accuracy도 떨어지면 어떻게 해야하나?
      학습기준을 loss에 맞춰야하는지 accuracy에 맞춰야 하는지 의문
      ```
      <br/>

      
- [x]  **4. 회고를 잘 작성했나요?**
    - 주어진 문제를 해결하는 완성된 코드 내지 프로젝트 결과물에 대해
    배운점과 아쉬운점, 느낀점 등이 기록되어 있는지 확인
    - 전체 코드 실행 플로우를 그래프로 그려서 이해를 돕고 있는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.

      <br/>

      > 회고를 통해 기대에 미치지 못한 일부 실험 결과에 대하여 원인을 분석하고자 하였다. 또한 프로젝트를 수행하면서 가진 의문점들을 제시하였다.

      <br/>

      <img width="599" alt="image" src="https://github.com/youungg/Aiffel_Quest/assets/149550222/617f353d-de1b-459a-b8b5-5e87c21c095e">
     
      ```
      학습기준을 loss에 맞춰야하는지 accuracy에 맞춰야 하는지 의문
      data 불균형 없애고 학습시키고 싶었는데 train_test_split 안돼서 못함 후,,,,,
      ```
      
      <br/>

- [x]  **5. 코드가 간결하고 효율적인가요?**
    - 파이썬 스타일 가이드 (PEP8) 를 준수하였는지 확인
    - 하드코딩을 하지않고 함수화, 모듈화가 가능한 부분은 함수를 만들거나 클래스로 짰는지
    - 코드 중복을 최소화하고 범용적으로 사용할 수 있도록 함수화했는지
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
          
      <br/>

      > 데이터 속성 등 변하지 않는 값에 대하여 대문자로만 구성된 변수명(상수 스타일의 변수)을 사용하는 등 통념적인 코딩 컨벤션에 부합하는 프로그래밍을 수행하였다.

      <br/>
     
      ```
      CAR_LIST = ['Car', 'Van', 'Truck', 'Tram']
      PEOPLE_LIST = ['Pedestrian', 'Person_sitting', 'Cyclist']
      ELSE_LIST = ['Misc']
      ```
      
      <br/>

# 참고 링크 및 코드 개선
```
# 코드 리뷰 시 참고한 링크가 있다면 링크와 간략한 설명을 첨부합니다.
# 코드 리뷰를 통해 개선한 코드가 있다면 코드와 간략한 설명을 첨부합니다.
```
