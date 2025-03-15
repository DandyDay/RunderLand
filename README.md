# Runderland

🏆 **[2023 메타버스 개발자 경진대회](https://www.metaversedev.kr/) 우수상 (한국퀄컴대표상) 수상작**  


## 🎥 데모 영상

<table align="center">
  <tr>
    <td align="center">
      <a href="https://www.youtube.com/watch?v=WNS9c8TE59s">
        <img src="https://img.youtube.com/vi/WNS9c8TE59s/0.jpg" alt="1차 시연영상" width="300"/>
      </a>
      <br>
      <b>1차 시연 영상</b>
    </td>
    <td align="center">
      <a href="https://www.youtube.com/watch?v=O8mDsIyQ21E">
        <img src="https://img.youtube.com/vi/O8mDsIyQ21E/0.jpg" alt="최종 시연영상" width="300"/>
      </a>
      <br>
      <b>최종 시연 영상</b>
    </td>
  </tr>
</table>

---

## 🏃 프로젝트 개요

**Runderland**는 실시간 위치 기반 러닝 메이트 기능과 네비게이션, 러닝 기록 저장 기능을 갖춘 메타버스 러닝 애플리케이션입니다.  

본 프로젝트에서 **팀장**으로서 전반적인 진행을 관리하며, 주요 기능의 설계 및 구현을 주도했습니다.

---

## 🔧 구현에 참여한 핵심 기능

### 1️⃣ 러닝 진행 방향 예측

러닝 메이트가 유저와 함께 자연스럽게 달릴 수 있도록, **유저의 실제 이동 방향을 예측하는 기능을 직접 개발**했습니다.  

🔹 **문제점:**  
기기 및 Unity에서 제공하는 위치·속도 정보에 노이즈가 많아 부정확함  

🔹 **해결 방법:**  
- **Queue 자료구조**를 활용하여 최근 10번의 측정 데이터를 저장하고, 평균 벡터를 유저의 진행 방향으로 설정  
- 비현실적인 속도를 보이는 데이터를 **무효(invalid) 값으로 필터링**  

```Csharp
if (movement > 0.025) 
{
    isValidMovement = true;
    directionVectorList.Enqueue(new Vector3(dxMov, 0, dzMov));
} 
else 
    isValidMovement = false;
```

- 사용자의 **급격한 방향 전환 감지**, 새로 입력된 속도 벡터에 가중치 적용  

```Csharp
if (Math.Abs(rot - prevRot) > 1.3) 
    directionVectorList.setArgument(0.6f); // Lerp with this argument
else 
    directionVectorList.setArgument(0.1f);
```

📌 **결과:**  
여러 실험을 통해 값을 수정하여 가장 자연스러운 진행 방향을 도출할 수 있도록 함.

---

### 2️⃣ 실시간 네비게이션 구현  

유저가 러닝 중 **자신의 위치를 쉽게 파악하고, 원하는 목적지로 이동할 수 있도록 네비게이션 기능을 직접 개발**했습니다.  

🔹 **주요 기능:**  
- 화면 우측 하단에 **실시간 미니맵 표시**  
- 사용자가 **목적지를 선택하면 최적 경로 제공**  
- **TMAP API** 활용하여 도보 길찾기 경로를 지도에 표시

<img width="725" alt="네비게이션 예시" src="https://github.com/user-attachments/assets/4ed03e21-3c33-4c32-bbf0-5c4fd1bb6655" />

📌 **결과:**

유저가 러닝 중 경로를 직관적으로 확인할 수 있어 편의성 향상.

---

### 3️⃣ 러닝 기록 GPX 파일 저장  

러너들이 자신의 운동 기록을 관리할 수 있도록 **GPX 파일로 러닝 데이터를 저장하는 기능을 직접 개발**했습니다.  

🔹 **주요 기능:**  
- 유저의 **GPS 이동 경로를 GPX 형식으로 저장**  
- 저장된 경로를 앱 내에서 확인 가능  
- 기록된 러닝 경로를 기반으로 **아바타와 함께 러닝 가능**  
- **GPX 표준 파일 형식 채택 → 다른 러닝 앱과 호환 가능**  

<img width="725" alt="러닝 기록 예시" src="https://github.com/user-attachments/assets/e524cc3b-a094-4061-a066-144912854cd9" />

📌 **결과:**  
러닝 기록을 체계적으로 관리할 수 있으며, 타 앱과의 연동성을 확보함.

---
