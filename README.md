### 이미지 데이터 제너레이터가 하는 역할
이미지 데이터 제너레이터는 이미지 증강을 통해 데이터를 부풀려 주는 역할도 하며, 0~255로 구성된 이미지의 숫자를 컴퓨터가 학습하기 쉬운 실수형태로 바꾸주는 역할도 한다.
ImageDataGenerator(rescale=1/255.0, rotation_range=40, width_shift_range=0.2, height_shift_range=0.2, shear_range=0.2, zoom_range=0.2, horizontal_flip=True, vertical_flip=True, fill_mode='nearest')
### 이미지 증강 기법
이미지 증강이란, 이미지의 크기, 밝기, 회전 등을 통해 학습 등의 데이터를 부풀려주는 활동을 말한다. 위의 ImageDataGenerator 코드를 이용하면 이미지 증강을 할 수 있다.
### 타임시리즈 데이터 분석용 Prophet 라이브러리 사용법
Prophet이란 페이스북에서 만든 오픈소스 라이브러리이며, from fbprophet import Prophet라이브러리 임포트를 하여 사용가능하다.
Prophet라이브러리를 사용하기 위해서는 문자형식으로 된 날짜를 pd.to_datetime을 통해 iso날짜 형식으로 바꿔줘야 한다. 또한, Columns은 날짜컬럼은 ds로, 예측하려는 수치의 컬럼은 y로 필수로 바꿔주어야 한다.
chicago_prophet.rename(columns={'Date':'ds',0:'y'}, inplace=True)
다음과 같이 사용한다.

m = Prophet()

m.fit(chicago_prophet)

future = m.make_future_dataframe(periods=30)

forecast = m.predict(future)

### 판다스 datetimeindex와 resample
데이터를 다루다 보면 날짜형식을 인덱스로 활용해야 할 경우가 많다. 그러한 경우 다음과 같은 방법을 통해 날짜형식을 iso시간 형태로 바꿔준 후 인덱스로 넘겨주면된다.

chicago_df['Date'] = pd.to_datetime(chicago_df['Date'], format = '%m/%d/%Y %I:%M:%S %p')

chicago_df.index = pd.DatetimeIndex(chicago_df['Date'])

또한 resample함수를 통해, 날짜를 얼마 간격으로 표시할지 설정 가능하다. 다음과 같이 사용한다.

year_df = chicago_df.resample('Y').size()
![image](https://user-images.githubusercontent.com/78472987/109779117-f7188180-7c48-11eb-9ba7-fbcbb82601ba.png)

