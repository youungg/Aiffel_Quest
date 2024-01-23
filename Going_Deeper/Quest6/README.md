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
    
    >문제에서 요구한대로 이미지로부터 텍스트 영역을 검출하고, 검출된 영역으로부터 문자를 인식해내는 목표를 성공적으로 달성하였다.

    <br/>
    
    <img width="624" alt="image" src="https://github.com/DiANA-KANG/Aiffel_Quest_LJY/assets/149550222/c2e97e41-b4a8-4c0f-a638-c5851de89c8d">
    <img width="624" alt="image" src="https://github.com/DiANA-KANG/Aiffel_Quest_LJY/assets/149550222/617b336b-ac0e-4692-a85a-eb7408ae28a2">

    <br/>
    <br/>

    
    
- [x]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 해당 코드 블럭에 doc string/annotation이 달려 있는지 확인
    - 해당 코드가 무슨 기능을 하는지, 왜 그렇게 짜여진건지, 작동 메커니즘이 뭔지 기술.
    - 주석을 보고 코드 이해가 잘 되었는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
     
    <br/>
    
    >이미지 데이터를 처리할 때, 이미지 파일로부터 배열 데이터 형태로 변환하는 과정, 배열 데이터를 예측과 화면 표시에 활용하기 위한 차원 변화 과정까지 알기 쉽게 주석으로 표시하였다.

    <br/>

    ```
    def recognize_img( pil_img, model_pred, input_img_size=(100,32)):
        # CRNN 기반의 Recognition 모델에 입력하기 위한 이미지 전처리
        # - 배치 크기 때문에 4 dims 로 확장
        # - shape=(W,H,C) 여야 모델 입력시 제대로 동작함
        pil_img = pil_img.resize(input_img_size)  # (100, 32, 3)
        pil_img = np.array(pil_img)  # (32, 100, 3)
        pil_img = pil_img.transpose(1,0,2)  # (100, 32, 3)
        pil_img = np.expand_dims(pil_img, axis=0)  # (1, 100, 32, 3)
        
        # 모델 예측
        # - OCR 텍스트 인식 결과 출력
        output = model_pred.predict(pil_img)
        result = decode_predict_ctc(output, chars="-"+TARGET_CHARACTERS).replace('-','')
        print("Result: \t", result)
        
        # display() 하기 위한 이미지 전처리
        # - 앞서 4 dims 로 확장시켰으므로 다시 3 dims 가 되도록 축소
        # - shape=(H,W,C) 여야 display() 에서 이미지가 제대로 그려짐
        pil_img = np.squeeze(pil_img, axis=0).transpose(1,0,2).astype(np.uint8)  # (32, 100, 3)
        pil_img = Image.fromarray(pil_img)
        display(pil_img)
    ```
    
    <br/>


        
- [x]  **3. 에러가 난 부분을 디버깅하여 문제를 “해결한 기록을 남겼거나” 
”새로운 시도 또는 추가 실험을 수행”해봤나요?**
    - 문제 원인 및 해결 과정을 잘 기록하였는지 확인
    - 문제에서 요구하는 조건에 더해 추가적으로 수행한 나만의 시도, 
    실험이 기록되어 있는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.


    <br/>
    
    >학습 epoch 별로 모델을 저장하고, epoch에 따른 학습 성능 결과를 비교하는 추가 실험을 시도하였다.

    <br/>

    ```
    # Define a custom callback to save model checkpoints at specific epochs
    class SaveModelCallback(tf.keras.callbacks.Callback):
        def on_epoch_end(self, epoch, logs=None):
            save_epochs = [10, 20, 30, 40, 50]
            if epoch in save_epochs:
                checkpoint_path = os.path.join(checkpoint_dir, f'model_checkpoint_{epoch}.hdf5')
                self.model.save_weights(checkpoint_path)
                print(f"Saved model checkpoint at epoch {epoch}")

    # ModelCheckpoint to save the best model based on validation loss
    model_checkpoint = tf.keras.callbacks.ModelCheckpoint(
        os.path.join(checkpoint_dir, 'best_model.hdf5'),
        monitor='val_loss', verbose=1,
        save_best_only=True, save_weights_only=True
    )
    
    # 학습을 진행합니다
    crnn_history = model.fit(
        train_set,
        steps_per_epoch=len(train_set),
        epochs=EPOCHS,
        validation_data=val_set,
        validation_steps=len(val_set),
        callbacks=[SaveModelCallback(), model_checkpoint]
    )
    ```
    
    <br/>

    
- [x]  **4. 회고를 잘 작성했나요?**
    - 주어진 문제를 해결하는 완성된 코드 내지 프로젝트 결과물에 대해
    배운점과 아쉬운점, 느낀점 등이 기록되어 있는지 확인
    - 전체 코드 실행 플로우를 그래프로 그려서 이해를 돕고 있는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.

    <br/>
    
    >실험 결과를 분석하거나, 문제 풀이하면서 궁금했던 점이나 배운 점을 간결하게 요약하여 기록하였다.

    <br/>

    ```
    rstrip 써도 9999 지울 수 있다. but gt 끝자리 9나와도 사라짐.-> 개선 완
    hermite, is right count, thermite로 나온거 결과 0임 -> 개선 방안 안떠오름
    transaction 사용 이유? mdb 파일 부를때 쓴다고 함. 맞나?
    transaction 쓸 때는 buf 많이 쓰는듯, 데이터 관리 및 속도 때문에
    np.array 쓰면 (W, H, C) -> (H, W, C) so, transpose(1,0,2)
    dimension 확장
        np_img = np.reshape((1,) + np_img.shape)
        np_img = np.expand_dims(np_img, axis=0)
        np_img = np_img[np.newaxis, :, :, :]
    dimension 축소
        np_img = np.squeeze(np_img, axis=0)
    detector = keras_ocr.detection.Detector()
    recognizer = keras_ocr.recognition.Recognizer() 이건 나중에 써보자
    ```
    
    <br/>


    
- [x]  **5. 코드가 간결하고 효율적인가요?**
    - 파이썬 스타일 가이드 (PEP8) 를 준수하였는지 확인
    - 하드코딩을 하지않고 함수화, 모듈화가 가능한 부분은 함수를 만들거나 클래스로 짰는지
    - 코드 중복을 최소화하고 범용적으로 사용할 수 있도록 함수화했는지
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
     
    <br/>
    
    >파이썬 스타일 가이드에 맞는 변수명을 사용하였고, 간결한 코드라인 길이를 유지함으로써 가독성을 향상시켰다.

    <br/>

    ```
    def detect_text(img_path):
    # 배치 크기를 위해서 dimension을 확장해주고 keras-ocr의 입력 차원에 맞게 H,W,C로 변경
    result_img = Image.open(img_path)
    img_draw = ImageDraw.Draw(result_img)
    img = result_img.copy()
    img_pil = np.array(result_img)[np.newaxis, :, :, :]
    
    # 배치의 첫 번째 결과만 가져옵니다.
    ocr_result = detector.detect(img_pil)[0]
    
    cropped_imgs = []
    
    for text_result in ocr_result:
        img_draw.polygon(text_result, outline='red')
        x_min = text_result[:,0].min() - 5
        x_max = text_result[:,0].max() + 5
        y_min = text_result[:,1].min() - 5
        y_max = text_result[:,1].max() + 5
        word_box = [x_min, y_min, x_max, y_max]
        cropped_imgs.append(img.crop(word_box))

    return result_img, cropped_imgs
    ```
    
    <br/>
    <br/>
    <br/>

# 참고 링크 및 코드 개선
```
# 코드 리뷰 시 참고한 링크가 있다면 링크와 간략한 설명을 첨부합니다.
# 코드 리뷰를 통해 개선한 코드가 있다면 코드와 간략한 설명을 첨부합니다.
```
