# HealthKit Background Delivery + 로컬 푸시 알림 구현 가이드

## 개요

Turtle Run 앱에서 사용자가 Apple 운동 앱이나 다른 러닝 앱으로 워크아웃을 완료했을 때, 앱이 백그라운드에 있거나 종료된 상태에서도 자동으로 Shell을 계산하고 푸시 알림을 보내는 기능 구현 방안입니다.

## 기술적 배경

### HealthKit Background Delivery란?

**HealthKit Background Delivery**는 앱이 백그라운드에 있거나 종료된 상태에서도 HealthKit 데이터 변경을 감지할 수 있게 해주는 iOS 기능입니다.

**작동 방식:**
1. 새로운 HealthKit 데이터(워크아웃 등)가 추가되면
2. iOS가 앱을 백그라운드에서 깨워줌  
3. 앱이 데이터를 처리하고 로컬 푸시 알림 발송

## 구현 방법

### 1. Background Delivery 활성화

```swift
// AppDelegate.swift - didFinishLaunchingWithOptions에서
healthStore.enableBackgroundDelivery(
    for: HKObjectType.workoutType(),
    frequency: .immediate
) { success, error in
    if success {
        print("Background delivery enabled for workouts")
    } else {
        print("Failed to enable background delivery: \(error?.localizedDescription ?? "Unknown error")")
    }
}
```

### 2. Observer Query 설정

```swift
let query = HKObserverQuery(
    sampleType: HKObjectType.workoutType(),
    predicate: nil
) { query, completionHandler, error in
    // 새 워크아웃 감지 시 Shell 계산
    self.processNewWorkout()
    
    // 로컬 푸시 알림 발송
    self.sendLocalNotification()
    
    // 완료 신호 (중요!)
    completionHandler()
}

healthStore.execute(query)
```

### 3. Turtle Run 적용 예제

```swift
// HealthKitManager.swift
class HealthKitManager {
    private let healthStore = HKHealthStore()
    
    func setupBackgroundDelivery() {
        // 워크아웃 종료 감지 설정
        healthStore.enableBackgroundDelivery(
            for: HKObjectType.workoutType(),
            frequency: .immediate
        ) { success, error in
            if success {
                self.setupWorkoutObserver()
            } else {
                print("Background delivery setup failed: \(error?.localizedDescription ?? "Unknown")")
            }
        }
    }
    
    private func setupWorkoutObserver() {
        let query = HKObserverQuery(
            sampleType: HKObjectType.workoutType(),
            predicate: nil
        ) { query, completionHandler, error in
            // 새 워크아웃 데이터 처리
            self.handleNewWorkout { 
                completionHandler()
            }
        }
        healthStore.execute(query)
    }
    
    private func handleNewWorkout(completion: @escaping () -> Void) {
        // 1. 최신 워크아웃 데이터 조회
        fetchLatestWorkout { workout in
            guard let workout = workout else {
                completion()
                return
            }
            
            // 2. Shell 계산
            let shellData = self.calculateShell(from: workout)
            
            // 3. 서버 전송
            self.sendShellToServer(shellData) { success in
                if success {
                    // 4. 로컬 푸시 알림 발송
                    self.sendShellNotification(shellSize: shellData.area)
                }
                completion()
            }
        }
    }
    
    private func sendShellNotification(shellSize: Double) {
        let content = UNMutableNotificationContent()
        content.title = "새로운 Shell 획득! 🐢"
        content.body = "러닝으로 \(Int(shellSize))㎡의 Shell을 획득했습니다"
        content.sound = .default
        
        let request = UNNotificationRequest(
            identifier: "new-shell-\(Date().timeIntervalSince1970)",
            content: content,
            trigger: nil
        )
        
        UNUserNotificationCenter.current().add(request) { error in
            if let error = error {
                print("Failed to send notification: \(error)")
            }
        }
    }
}
```

## 필수 설정

### 1. Entitlement 추가
- `com.apple.developer.healthkit.background-delivery`

### 2. Background Modes 활성화
- Target → Capabilities → Background Modes
- "Background processing" 체크

### 3. Info.plist 설정
```xml
<key>UIBackgroundModes</key>
<array>
    <string>background-processing</string>
</array>
```

### 4. HealthKit 권한 요청
```swift
let healthTypes: Set<HKSampleType> = [
    HKObjectType.workoutType(),
    HKObjectType.quantityType(forIdentifier: .distanceWalkingRunning)!,
    // 기타 필요한 데이터 타입들
]

healthStore.requestAuthorization(toShare: nil, read: healthTypes) { success, error in
    if success {
        self.setupBackgroundDelivery()
    }
}
```

## 주요 제한사항

### 1. 디바이스 잠금 시 제한
- **문제**: 디바이스가 잠긴 상태에서는 HealthKit 데이터를 읽을 수 없음
- **해결**: 데이터 쓰기는 가능하며, 잠금 해제 시 캐시된 데이터가 처리됨
- **영향**: Shell 계산이 디바이스 잠금 해제 시점까지 지연될 수 있음

### 2. 업데이트 빈도 제한
- **걸음 수**: 최소 1시간 간격으로만 업데이트
- **워크아웃**: `immediate` 설정 가능 (실시간에 가까움)
- **Turtle Run**: 워크아웃 데이터 사용으로 실시간 처리 가능

### 3. 개발 및 테스트 제한
- **시뮬레이터**: Background delivery 미지원
- **테스트**: 실제 디바이스에서만 정상 작동
- **지연**: 약간의 지연 시간 존재 (수 분 내외)

### 4. iOS 시스템 제한
- **배터리**: 배터리 절약 모드 시 제한될 수 있음
- **시스템 리소스**: iOS가 시스템 리소스 상황에 따라 백그라운드 실행 제한 가능

## 구현 시 고려사항

### 1. 에러 처리
```swift
private func handleNewWorkout(completion: @escaping () -> Void) {
    do {
        // Shell 계산 로직
    } catch {
        // 에러 로깅
        print("Shell calculation failed: \(error)")
        // 재시도 로직 또는 다음 실행 시 처리
    }
    
    // 반드시 completion 호출
    completion()
}
```

### 2. 중복 처리 방지
```swift
// 이미 처리된 워크아웃인지 확인
private func isWorkoutAlreadyProcessed(_ workout: HKWorkout) -> Bool {
    let processedWorkouts = UserDefaults.standard.array(forKey: "processedWorkouts") as? [String] ?? []
    return processedWorkouts.contains(workout.uuid.uuidString)
}

private func markWorkoutAsProcessed(_ workout: HKWorkout) {
    var processedWorkouts = UserDefaults.standard.array(forKey: "processedWorkouts") as? [String] ?? []
    processedWorkouts.append(workout.uuid.uuidString)
    UserDefaults.standard.set(processedWorkouts, forKey: "processedWorkouts")
}
```

### 3. 네트워크 연결 고려
```swift
private func sendShellToServer(_ shellData: ShellData, completion: @escaping (Bool) -> Void) {
    // 네트워크 연결 확인
    guard NetworkReachability.isConnected else {
        // 로컬에 저장 후 나중에 전송
        saveShellDataLocally(shellData)
        completion(false)
        return
    }
    
    // 서버 전송 로직
}
```

## 테스트 방법

### 1. 실제 디바이스 테스트
1. Xcode에서 실제 디바이스에 앱 설치
2. HealthKit 권한 허용
3. Apple 운동 앱 또는 다른 러닝 앱으로 짧은 워크아웃 실행
4. 워크아웃 종료 후 알림 확인

### 2. 로그 확인
```swift
// 백그라운드 실행 로그 추가
private func handleNewWorkout(completion: @escaping () -> Void) {
    print("🔵 Background workout processing started at \(Date())")
    
    // 처리 로직...
    
    print("🟢 Background workout processing completed at \(Date())")
    completion()
}
```

## 대안 방안

### 1. 서버 기반 폴링
- HealthKit 데이터를 주기적으로 서버에서 확인
- Apple Health Records API 활용 (제한적)
- 서버에서 새 데이터 감지 시 APNs 푸시 알림

### 2. Shortcuts 앱 연동
- 사용자가 워크아웃 종료 시 Shortcuts 앱으로 Turtle Run 호출
- 수동적이지만 확실한 방법

## 관련 문서

### Apple 공식 문서
- [enableBackgroundDelivery](https://developer.apple.com/documentation/healthkit/hkhealthstore/1614175-enablebackgrounddelivery)
- [HKObserverQuery](https://developer.apple.com/documentation/healthkit/hkobserverquery)
- [Background Delivery Entitlement](https://developer.apple.com/documentation/bundleresources/entitlements/com.apple.developer.healthkit.background-delivery)

### 커뮤니티 리소스
- [Stack Overflow - HealthKit Background Delivery](https://stackoverflow.com/questions/25088956/how-to-use-healthkit-background-delivery)
- [GitHub Example Code](https://gist.github.com/phatblat/654ab2b3a135edf905f4a854fdb2d7c8)

## 결론

HealthKit Background Delivery를 통해 Turtle Run 앱이 종료되어도 새로운 러닝 세션 완료 시 자동으로 Shell을 계산하고 사용자에게 알림을 보내는 것이 가능합니다. 

**장점:**
- 사용자 편의성 극대화
- 자동화된 Shell 계산
- 실시간성 제공

**단점:**
- 구현 복잡도 증가
- 디바이스 잠금 시 제한
- 시뮬레이터 테스트 불가

전반적으로 Turtle Run의 핵심 사용자 경험을 향상시키는 중요한 기능으로 판단되며, 제한사항들을 고려한 견고한 구현이 필요합니다.