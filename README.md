# 📌 Architecture

<img src="./architecture.jpg"/>

<br><br><br><br><br>


# 📌 API 명세

| No | 기능 | Method | URI | 설명 |
| :-- | :--- | :---: | :--- | :--- |
| 1 | **디바이스 리스트 조회** | `GET` | `/api/deviceList` | 전체 디바이스 목록 및 프로토콜 정보 조회 |
| 2 | **특정 디바이스 하위 태그** | `GET` | `/api/device/{deviceName}` | 특정 디바이스에 속한 태그 리스트 조회 |
| 3 | **특정 태그 조회** | `GET` | `/api/tag/{tagName}` | 특정 태그의 상세 정보 조회 |
| 4 | **알람 발생 태그 조회** | `GET` | `/api/tag/{tagName}` | 알람이 발생한 태그의 상세 정보 조회 |

<br><br><br>

### 1️⃣ 디바이스 리스트 조회
전체 디바이스의 목록과 통신 프로토콜 정보를 반환합니다.

- **Method:** `GET`
- **URI:** `/api/deviceList`

#### **Response**

| 필드명 | 타입 | 설명 |
| :--- | :--- | :--- |
| `deviceList` | Array | 디바이스 목록 배열 |
| └ `name` | String | 디바이스 명 (예: AHU1) |
| └ `description` | String | 디바이스 설명 |
| └ `protocol` | String | 통신 프로토콜 (예: BACnet) |
| └ `content1` | String | IP 주소 |
| └ `content2` | String | Port 번호 |
| └ `content3` | String | Unit ID / Network ID 등 |

**Example Code:**
```json
{
  "deviceList": [
    {
      "name": "AHU1",
      "description": "1",
      "protocol": "BACnet",
      "content1": "192.168.30.1",
      "content2": "47808",
      "content3": "7701"
    }
  ]
}
```

<br><br><br>

### 2️⃣ 특정 디바이스의 하위 태그 조회
특정 디바이스(`deviceName`)에 포함된 하위 태그 목록을 조회합니다.

- **URL:** `/api/device/{deviceName}`
- **Method:** `GET`
- **Path Variables:**
  - `deviceName`: 조회할 디바이스 명 (예: `AHU1`)

#### **Response**

| Field | Type | Description |
| :--- | :--- | :--- |
| `{deviceName}` | Array | **[Dynamic Key]** 요청한 디바이스 명을 Key로 하는 배열 |
| └ `name` | String | 태그 명 (예: TAG1) |
| └ `description` | String | 태그 설명 |
| └ `type` | String | 데이터 타입 (예: Analog, Digital) |
| └ `value` | Number | 현재 값 |
| └ `isAlarm` | Integer | 알람 상태 (0: 정상, 1: 알람) |

#### **Example Code**
```json
{
  "AHU1": [
    {
      "name": "TAG1",
      "description": "1층 공조기",
      "type": "Analog",
      "value": 5.5,
      "isAlarm": 1
    }
  ]
}
```

<br><br><br>

### 3️⃣ 특정 태그 조회
특정 태그(`tagName`)의 상세 정보와 현재 값을 조회합니다.

- **URL:** `/api/tag/{tagName}`
- **Method:** `GET`
- **Path Variables:**
  - `tagName`: 조회할 태그 명 (예: `TAG1`)

#### **Response**

| Field | Type | Description |
| :--- | :--- | :--- |
| `{tagName}` | Object | **[Dynamic Key]** 요청한 태그 명을 Key로 하는 객체 |
| └ `name` | String | 태그 명 |
| └ `description` | String | 태그 설명 |
| └ `type` | String | 데이터 타입 |
| └ `value` | Number | 현재 값 |
| └ `isAlarm` | Integer | 알람 상태 |

#### **Example Code**
```json
{
  "TAG1": {
    "name": "TAG1",
    "description": "1층 공조기",
    "type": "Analog",
    "value": 5.5,
    "isAlarm": 1
  }
}
```

<br><br><br>

### 4️⃣ 알람 발생 태그 조회
알람 상태인 태그의 상세 정보(디바이스 정보 포함)를 반환합니다.

- **URL:** `/api/tag/{tagName}`
- **Method:** `GET`
- **Path Variables:**
  - `tagName`: 조회할 태그 명

#### **Response**

| Field | Type | Description |
| :--- | :--- | :--- |
| `tagList` | Object | 알람 태그 정보 객체 |
| └ `deviceName` | String | 소속 디바이스 명 |
| └ `deviceDescription`| String | 소속 디바이스 설명 |
| └ `tagName` | String | 태그 명 |
| └ `tagDescription` | String | 태그 상세 설명 |
| └ `type` | String | 데이터 타입 |
| └ `value` | Number | 알람 발생 시점의 값 |
| └ `isAlarm` | Integer | 알람 상태 (1: 알람) |

#### **Example Code**
```json
{
  "tagList": {
    "deviceName": "AHU1",
    "deviceDescription": "공조기1",
    "tagName": "TAG1",
    "tagDescription": "1층 공조기의 온도설정",
    "type": "Analog",
    "value": 5.5,
    "isAlarm": 1
  }
}
```
