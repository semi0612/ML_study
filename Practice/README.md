# ๐ฑ Code Practice

์์  ์ค์ต ์ค ๋๋ฆ๋๋ก ์ ๋ฆฌํด๋ณด๊ณ  ์ถ์ ๊ฒ๋ค, ํด๋ณด๊ณ  ์ถ์ ๊ฒ๋ค์ ์ค์ตํด๋ณด๊ณ  ๊ธฐ๋กํด ๋๋ ๊ณณ์๋๋ค.


## cat_vs_dog(mini_dataset)
https://github.com/semi0612/study/blob/main/Practice/cat_vs_dog(mini_dataset).ipynb
### ๋ชจ๋ธ 7๊ฐ์ง
- ๋น๊ตํ  ๋ชจ๋ธ 7๊ฐ์ง
  - VGG16, VGG19, Inception_v3(GoogleNet), ResNet, Xception, MobileNet, DenseNet
- ๋น๊ตํ  ๊ฒ
  - [ train loss - validation loss / train accaurcy - validation accaurcy ] ๊ทธ๋ํ<br>
  - confusion metrics
- EarlyStopping callbacks๋ ์ฌ์ฉ(๋จ, ๋์ผ ์ค์ )
- ๋ชจ๋ธ์ ๋ถ๋ฌ์ ์ถ๊ฐํ  ๋ชจ๋ธ ์ฝ๋/compile ์ค์ ์ ์๋์ ๋์ผํ๋ค

```python
inputs = keras.Input(shape=(180, 180, 3))
x = data_augmentation(inputs)
x = keras.applications.vgg19.preprocess_input(x)
x = '๋ถ๋ฌ์จ ๋ชจ๋ธ'(x)
x = layers.Flatten()(x)
x = layers.Dense(256)(x)
x = layers.Dropout(0.5)(x)
outputs = layers.Dense(1, activation='sigmoid')(x)
'์๋ก ์์ฑ๋ ๋ชจ๋ธ ๋ช' = keras.Model(inputs, outputs)


'์๋ก ์์ฑ๋ ๋ชจ๋ธ ๋ช'.compile(loss='binary_crossentropy',
              optimizer='rmsprop',
              metrics='acc')
```

### ๊ฒฐ๊ณผ
ํฐ ์ฐจ์ด์๋ ๊ฒฐ๊ณผ๊ฐ ๋์๋ค.<br>
์์ ๋ฐ์ดํฐ๋ฅผ ์ฌ์ฉํด์ ๊ทธ๋ด ์๋ ์๋ค๋ ์๊ฐ์ด ๋ค์ด ์ถํ ๋ ํฐ ๋ฐ์ดํฐ๋ก ๋ชจ๋ธ๋ผ๋ฆฌ์ ๋น๊ต๋ฅผ ํด๋ณด๊ณ  ์ถ์ด์ง๋ค.
