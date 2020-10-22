시놀로지 나스(Synology Nas)의 비디오 스테이션(Video Station)에선 라이선스 문제 때문에 **DTS, EAC3, TrueHD** 영상을 재생할 수 없다. FFmpeg 패키지를 설치하는 방법 역시 비디오 스테이션 2.3.4 버전 이상으로 업데이트하면 재생할 수 없다. ▼

![https://blog.kakaocdn.net/dn/nLMYN/btqCZ029wUx/K3xcdnBODdBhBHgE6A84e0/img.png](https://blog.kakaocdn.net/dn/nLMYN/btqCZ029wUx/K3xcdnBODdBhBHgE6A84e0/img.png)

비디오 스테이션에서 DTS 영상을 재생했을 때 화면

다행히 비디오 스테이션을 이전 버전으로 유지하는 것보다 더 좋은 해결방법이 있다. Github 유저([Benjamin Poncet](https://gist.github.com/BenjaminPoncet))가 만든 [FFmpeg Wrapper](https://gist.github.com/BenjaminPoncet/bbef9edc1d0800528813e75c1669e57e) 스크립트를 설치하면 된다. 이 스크립트는 DSM 폴더 수정 권한을 요구하므로 SSH로 접속하여 설치해야 한다. SSH 접속도 아래 가이드에 따라 천천히 따라 하면 전혀 어렵지 않다. **설치 가능한 나스 모델명과 호환하는 DSM, 비디오스테이션 버전은 아래와 같다.**

- **가능한 나스 모델명**
    - **x64/x86** 아키텍처 : DS218+, DS718+, DS918+, DS418play, DS1019+ 등
    - **RTD1296 ARMv8** 아키텍처 : DS418j, DS418, DS218, Ds218play DS118, RS819
- **DSM :** DSM 6.2.2
- **Video Station 버전 :** 2.4.6
- **FFmpeg 패키지 버전 :** 4.2.1-23

❗️ 2.4.7 버전 이상의 Video Station 업데이트가 있다면 스크립트를 재설치해줘야 한다.

## 1. FFmpeg 패키지 설치

---

### **1) 패키지 설치 신뢰 수준 변경**

DSM → 패키지 센터 → 우측 상단 [설정] 버튼 클릭 → [일반] 탭에서 **신뢰 수준** 항목의 **Synology Inc. 및 신뢰할 수 있는 게시자** 체크 ▼

![https://blog.kakaocdn.net/dn/baC58v/btqCZzdFdOP/3TB5KnwbLrYlk9UYQDtqhK/img.png](https://blog.kakaocdn.net/dn/baC58v/btqCZzdFdOP/3TB5KnwbLrYlk9UYQDtqhK/img.png)

### **2) 패키지 소스 추가**

DSM → 패키지 센터 → 우측 상단 [설정] 버튼 클릭 → [패키지 소스] 탭에서 추가 버튼을 누른 뒤 아래와 같이 입력한다. ▼

![https://blog.kakaocdn.net/dn/1HjQE/btqC0K6QyHu/beZejgkLxziRgQdGs5QUkk/img.png](https://blog.kakaocdn.net/dn/1HjQE/btqC0K6QyHu/beZejgkLxziRgQdGs5QUkk/img.png)

- 이름 : 임의 입력
- 위치 : [http://packages.synocommunity.com](http://packages.synocommunity.com/)

### **3) FFmpeg 패키지 설치**

패키지 센터 좌측 [커뮤니티]를 클릭한 뒤 검색어에 FFmpeg 입력하여 설치. ▼

![https://blog.kakaocdn.net/dn/PRD9Z/btqC1HWnGcf/dmrWLqynZ1yoGE9cnrJtn1/img.png](https://blog.kakaocdn.net/dn/PRD9Z/btqC1HWnGcf/dmrWLqynZ1yoGE9cnrJtn1/img.png)

## 2. 나스 SSH 활성화

---

### **1) DSM의 SSH 서비스 활성화**

DSM → 제어판 → 터미널 및 SNMP에서 **SSH 서비스 활성화** 체크. 22번 기본 포트는 보안 위협이 있으므로 다른 임의 포트로 변경. ▼

![https://blog.kakaocdn.net/dn/dLr1mx/btqCX8t8qdP/8gox1e12Ja4qzpwM8P0aY1/img.png](https://blog.kakaocdn.net/dn/dLr1mx/btqCX8t8qdP/8gox1e12Ja4qzpwM8P0aY1/img.png)

### **❗️ (참고) 외부 접속 시 SSH 포트 포트 포워딩**

만약 외부 네트워크에서 접속한다면 SSH 포트에 대한 포트 포워딩이 필요하다. **"프로토콜"**은 **TCP**로 선택하고, **"내부 포트"**는 DSM에서 설정한 SSH 포트를 입력하면 된다. **"외부 포트"**는 외부 네트워크에서 SSH 접속 시 입력하는 포트다. 내부 포트와 같아도 되고 다르게 설정해도 된다. **"내부 IP 주소"**는 시놀로지 나스의 내부 IP주소를 입력한다 ▼

![https://blog.kakaocdn.net/dn/t2Yw5/btqCX79TMen/l7JjDLLey6I4vNut0tgFS0/img.png](https://blog.kakaocdn.net/dn/t2Yw5/btqCX79TMen/l7JjDLLey6I4vNut0tgFS0/img.png)

ASUS 공유기 포트포워딩 예시

나스의 내부 IP 주소는 DSM에 접속한 뒤 바탕화면 우측 상단 시스템 상태의 **LAN 포트**에서 확인할 수 있다 ▼

![https://blog.kakaocdn.net/dn/G5Cra/btqCYETQbdK/iwDgvdMZw6tT7WLKujcfW0/img.png](https://blog.kakaocdn.net/dn/G5Cra/btqCYETQbdK/iwDgvdMZw6tT7WLKujcfW0/img.png)

## 3. SSH 접속

---

### **1) [Mac OS] 맥 터미널을 통한 접속**

```
ssh 로그인ID@나스IP주소 -p SSH포트
```

- **로그인 ID :** DSM 로그인 ID
- **나스 IP 주소 :** 나스 내부 IP 주소

    ex) 192.168.1.12

- **SSH 포트 :** DSM에서 입력한 SSH 접속 포트

    ex) 2122

위 명령어 입력 후 비밀번호를 입력해야 하는데 **나스 DSM 로그인 계정과 동일한 패스워드**를 입력해주면 된다. 외부 네트워크에서 접속할 땐 나스 IP 주소에 DDNS 주소를 입력해야 한다.

### **2) [Windows] Putty를 통한 접속**

윈도우 운영체제는 [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)라는 프로그램을 이용하면 편리하다. Putty 실행 후 Session 화면에서 아래와 같이 입력한다. ▼

![https://blog.kakaocdn.net/dn/WrDAC/btqC2tjlvDy/LWxfjSKCVgLlfiYSJi6XCK/img.png](https://blog.kakaocdn.net/dn/WrDAC/btqC2tjlvDy/LWxfjSKCVgLlfiYSJi6XCK/img.png)

- **Host Name :** 로그인 ID@나스IP주소

    ex) romantech@192.168.1.12

- **Port :** SSH 포트

    ex) 2122

Host Name, Port 입력 후 하단 "Open" 버튼을 누르면 경고창이 나온다. 무시하고 확인(Yes) 버튼을 누르자. 그럼 검은색의 터미널 화면이 뜨면서 패스워드를 입력하라고 나온다. 여기에 나스 DSM 로그인 계정과 동일한 비밀번호를 입력하면 된다.

## 4. (SSH) Root 계정 접속

---

DSM 폴더, 파일 수정 등의 권한이 필요하므로 Root 계정 접속이 필요하다. 터미널에 아래 명령어를 입력한다. 입력 후 DSM 로그인 계정과 동일한 비밀번호를 입력한다. ▼

```
sudo -i
```

## 5-1. FFmpeg Wrapper 스크립트 설치 : x64, x86 아키텍처 모델

---

FFmpeg Wrapper 스크립트는 나스 모델에 따라 설치 방법이 조금씩 다르다. DS218+, DS718+, DS918+ 등 **x64** 및 **x86** 아키텍처 모델은 아래 명령어만 입력하면 된다(자신이 사용하는 나스 기종의 아키텍처는 [링크](https://github.com/SynoCommunity/spksrc/wiki/Architecture-per-Synology-model)에서 확인할 수 있다).

```
mv -n /var/packages/VideoStation/target/bin/ffmpeg /var/packages/VideoStation/target/bin/ffmpeg.orig
wget -O - https://gist.githubusercontent.com/BenjaminPoncet/bbef9edc1d0800528813e75c1669e57e/raw/ffmpeg-wrapper > /var/packages/VideoStation/target/bin/ffmpeg
chown root:VideoStation /var/packages/VideoStation/target/bin/ffmpeg
chmod 750 /var/packages/VideoStation/target/bin/ffmpeg
chmod u+s /var/packages/VideoStation/target/bin/ffmpeg
cp -n /var/packages/VideoStation/target/lib/libsynovte.so /var/packages/VideoStation/target/lib/libsynovte.so.orig
chown VideoStation:VideoStation /var/packages/VideoStation/target/lib/libsynovte.so.orig
sed -i -e 's/eac3/3cae/' -e 's/dts/std/' -e 's/truehd/dheurt/' /var/packages/VideoStation/target/lib/libsynovte.so

```

## 5-2. FFmpeg Wrapper 스크립트 설치 : RTD1296 ARMv8 아키텍처 모델

---

DS418, DS218, DS118 등 **RTD1296 ARMv8** 아키텍처 모델은 명령어를 입력하기 전 몇 가지 작업을 더 수행해야 한다.

- Video Station 삭제 ("데이터 삭제하기"에 체크하지 않는다면 재설치 시 기존 설정 및 메타데이터 그대로 보존)
- Video Station 2.3.4 버전 다운로드 [(링크)](https://archive.synology.com/download/Package/spk/VideoStation/2.3.4-1468/)
- 다운로드한 SPK 파일을 DSM 패키지 센터에서 수동 설치
- 아래 명령어 입력 ▼

```
cp -a /var/packages/VideoStation/target/lib/ffmpeg /tmp/
```

- DSM 패키지 센터에서 Video Station 업데이트
- 아래 명령어 입력 ▼

```
mv -n /var/packages/VideoStation/target/lib/ffmpeg /var/packages/VideoStation/target/lib/ffmpeg.orig
mv /tmp/ffmpeg /var/packages/VideoStation/target/lib/
cp -n /var/packages/VideoStation/target/lib/libsynovte.so /var/packages/VideoStation/target/lib/libsynovte.so.orig
chown VideoStation:VideoStation /var/packages/VideoStation/target/lib/libsynovte.so.orig
sed -i -e 's/eac3/3cae/' -e 's/dts/std/' -e 's/truehd/dheurt/' /var/packages/VideoStation/target/lib/libsynovte.so

```

드디어 설치가 완료됐다. DSM에서 Video Station을 다시 실행하면 DTS, EAC3 등의 영상도 잘 작동되는 걸 확인할 수 있다.

## 🔎 FFmpeg Wrapper 스크립트 삭제

---

스크립트 삭제는 아래 해당하는 아키텍처의 명령어를 터미널에 입력해주면 된다(나스 Root 계정 접속 상태에서 입력).

- x64, x86 아키텍처 (DS218+, DS718+, DS918+ 등) ▼

```
mv -f /var/packages/VideoStation/target/bin/ffmpeg.orig /var/packages/VideoStation/target/bin/ffmpeg
mv -f /var/packages/VideoStation/target/lib/libsynovte.so.orig /var/packages/VideoStation/target/lib/libsynovte.so

```

- rtd1296 armv8 아키텍처 (DS418, DS218, DS118 등) ▼

```
rm -f /var/packages/VideoStation/target/lib/ffmpeg
mv -f /var/packages/VideoStation/target/lib/ffmpeg.orig /var/packages/VideoStation/target/lib/ffmpeg
mv -f /var/packages/VideoStation/target/lib/ffmpeg.orig /var/packages/VideoStation/target/lib/ffmpeg

```

## 🔎 스크립트 업데이트

---

현재 FFmpeg Wrapper 스크립트는 **12** 버전이다. 추후 새로운 버전의 스크립트가 나왔을 땐 아래 명령어를 통해 업데이트할 수 있다. (x64, x86 아키텍처만 해당)

```
wget -O - https://gist.githubusercontent.com/BenjaminPoncet/bbef9edc1d0800528813e75c1669e57e/raw/ffmpeg-wrapper > /var/packages/VideoStation/target/bin/ffmpeg
```

## 🔎 Video Station 업데이트 시

---

만약 비디오 스테이션 버전 업데이트가 있다면 FFmpeg Wrapper를 재설치해야 한다. 스크립트 제작자가 비디오 스테이션 2.4.6 버전을 기준으로 만들어서 발생하는 문제로 보인다. (비디오 스테이션 2.4.6 버전에서 스크립트 설치 → **정상 작동** → 2.4.7 버전 업데이트 → **작동 안 함** → 스크립트 재설치 → **정상 작동**)
