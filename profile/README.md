# 🏠 GongSiRi(공시리)

# 목차
[1. 서비스 소개](#1._서비스_소개) <br>
[2. 아키텍쳐](#2._아키텍쳐) <br>
[3. 주요기능](#3._주요_기능) <br>
[4. 실행 방법](#4._실행_방법) <br> 
[5. Convention](#5._convention) <br>
[6. 팀 소개](#6._팀_소개) <br>

# 1. 서비스 소개
![image](https://github.com/Gong-siri/GongSiRi_Front/assets/86208370/356ec331-0c98-418d-bd5c-2f9624b47cc6)
공실이는 단기임대의 비효율을 해결하는 플랫폼입니다. <br>
팬데믹이 끝난 이후 상권들이 점차 회복되고 있지만 주요상권의 공실률이 꾸준히 증가하고 있습니다. <br>
공실을 해결 할 수 있는 방법으로 팝업스토어 시장이 크게 성장하고 있습니다. <br>
하지만 단기임대차 계약 과정은 여전히 비효율적입니다. 


# 2. 아키텍쳐
![image](https://github.com/Gong-siri/GongSiRi_Front/assets/86208370/0722f126-a3ea-4a23-b2db-9d521a485435)

# 3. 주요 기능

## ✅ 회원가입, 로그인
![로그인](https://github.com/Gong-siri/GongSiRi_Front/assets/86208370/cbf6d8d1-c631-45dd-ba26-fe23dda6e321)
로그인 인증, 인가를 `JWT 토큰 방식`으로 개발했습니다. <br>
API를 Instance로 모듈화하여, API를 용이하게 관리했습니다. <br>

```js
// 회원관리, 로그인 API
import axiosInstance from './instance';

export const signUp = async (user) => {
  return axiosInstance.post('/auth/signup', { ...user });
};

export const signIn = async (user) => {
  return axiosInstance.post('/auth/signin', { ...user });
};
```


```js
// API Instance화
import axios from 'axios';
import { ACCESS_TOKEN } from '../constant/authConstant';

const axiosConfig = {
  timeout: 3000,
  baseURL: process.env.REACT_APP_API_URL,
};

const axiosInstance = axios.create(axiosConfig);

axiosInstance.interceptors.request.use(
  (config) => {
    if (!config.headers) config.headers = {};
    config.headers['Content-Type'] = 'application/json';
    config.headers['X-Requested-With'] = 'XMLHttpRequest';
    config.headers.Authorization = `Bearer ${localStorage.getItem(
      ACCESS_TOKEN,
    )}`;
    config.headers.Accept = 'application/json';
    return config;
  },
  (error) => Promise.reject(error),
);

axiosInstance.interceptors.response.use(
  (response) => response,
  (error) => {
    const { response } = error;
    if (response.status === 401) return Promise.reject(error);
    if (response) alert(response.data.message);
    return Promise.reject(error);
  },
);

export default axiosInstance;
```

<br>

## ✅ Admin Console(대시보드 매물 관리)
![image](https://github.com/Gong-siri/GongSiRi_Front/assets/86208370/fb056f42-eede-497b-8a7d-843a081f8981)
임대계약 진행 상황, 임대현황, 나의 매물을 쉽게 관리할 수 있도록 UI/UX를 제공합니다<br> 
임대계약 진행 상황은 새요청, 검토 중, 계약 진행 중, 계약 완료 단계로 구성됩니다.

<br>

## ✅ 나의 매물
![대시보드](https://github.com/Gong-siri/GongSiRi_Front/assets/86208370/0e945778-5a9b-4619-8a3d-4da0f2d43e49)
대시보드 화면에서 매물 게시 정보 변경을 통해 이동합니다. <br> 
매물의 임대료 변경, 부가시설 변경, 매물 대표 이미지 변경을 통해 유저에게 매물의 정보를 제공합니다.

## ✅ 매물 시세 분석
![시세분석](https://github.com/Gong-siri/.github/assets/86208370/31fe96cd-5b29-4ea0-89ca-80015aabed02)
![image](https://github.com/Gong-siri/.github/assets/86208370/d7ead6f2-4883-4208-88d3-1f0f559068de)
주소(위도, 경도), 규모, 공실률, 순영업소득, 건물 층, 임대료를 활용합니다. <br>
데스크 리서치를 통해 찾은 논문으로 임대료 예측 시 필요한 변수 후보들을 추출했습니다. <div>


<br>

# 4. 실행 방법

### 로컬 서버 구동

- 배포된 API에 문제가 있는 경우 활용할 수 있는 로컬 서버 구동법입니다.
- 로컬 서버는 sqlite에 의존성이 있습니다.

### 설치 및 실행

```zsh
$ npm install
$ npm start
```

- 위 순서대로 실행하면 localhost:8000 포트에 서버가 실행됩니다.
- 서버를 실행하면 db.sqlite 파일이 생성되며 해당 파일을 삭제 시 기존의 데이터는 초기화 됩니다.
  
</br>

# 5. convention

### **git Flow**

- branch : 기능별 작업
- main(master) : 최종 배포

### **Commit Message Pre-fix**

- feat: 새로운 기능 추가
- fix: 버그 수정
- docs: 내부 문서 추가/수정
- style: 코드 스타일 수정
- refactor: 코드 리팩토링
- test: 테스트 추가/수정
- chore: 빌드 관련 코드 수정

## 6. 팀 소개
<br>

⚽ **팀장 김민아**

* Front-end & ML
* 매물 시세 분석
* Admin Console(대시보드 매물 관리) 개발

🐰 **이예나**

* Front-end & ML
* 매물 시세 분석
* 나의 매물 상세 페이지 개발

 🐨 **정진홍**

* Front-end
* 나의 매물 상세 페이지 개발

🐼 **권성호**

* Front-end & Back-end
* DB 
  

🐹 **김승연**

* 기획
* 피그마 

🐶 **지영민**

* 기획


⚽ **윤주호**
* 기획

