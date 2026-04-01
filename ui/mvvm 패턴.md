> ## 구조
_View (UI) -> (이벤트 전달) ->
ViewModel (중간 처리) ->
(데이터 요청) ->
Model (데이터 / 서버 / DB)_

----

> ## 기본 구조


- 로직 :
_조건문 (if) /
계산 (++, 합계) /
데이터 가공 /
API 호출_

    - View (UI) 
        - 특징 로직 없음 / ViewModel만 호출

    - ViewControllor
        - ####  _안드로이드에서는 Activity / Composable / 이벤트 처리 부분이 ViewController 역할_
        - View를 어떻게 동작하게 만들지 제어
        - View 안에서 ViewModel을 호출하는 과정
        
     ```kotlin
        onClick { }
        TextField { onValueChange }
        setContent { }
     ```
    
    -> Composable 내부 이벤트 처리 부분 + viewModel 호출

    - ViewModel 
        - ___상태(state) 저장 / 로직 처리 / 데이터 가공(데이터만 관리) / 모델 연결___
        - 상태 종류 

        ->  ___버튼 클릭 횟수 /
입력창 글자 /
로그인 성공 여부 /
서버에서 받아온 사용자 이름 /
로딩 중인지 여부___

    - Model 
        - 데이터
            - 서버(API) / DB / Repository
----

> ## __실제 작동 흐름__

1. 앱 시작
→ Activity 실행
→ setContent { Screen(viewModel) }

2. UI 처음 그려짐
→ ViewModel의 상태를 읽음

3. 사용자 이벤트 발생 (버튼 클릭)
→ View → viewModel 함수 호출

4. ViewModel에서 로직 실행
→ 상태 변경 (mutableStateOf / StateFlow)

5. Compose가 상태 변경 감지
→ recomposition 발생

6. UI 자동 업데이트

---
> # 정리
MVVM은 구조(설계 방식)
- UI와 로직을 분리하는 아키텍처 패턴

    - ___main을 비유 시 ViewModel 클래스를 만들고 사용자가 View에서 입력 시 ViewModel이 model이나 다른 것들을 이용해 처리___

- 간단한 실제 구조
1. 앱 실행
2. Activity 생성 (onCreate)
3. setContent 실행
4. MVVM 구조 안에서 UI 실행
5. 사용자 클릭
6. ViewController → ViewModel
7. 상태 변경 → UI 갱신

---
> ## 예시
- ViewModel

```kotlin
import androidx.lifecycle.ViewModel
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.StateFlow

class LoginViewModel : ViewModel() {

    private val _loginState = MutableStateFlow(false)
    val loginState: StateFlow<Boolean> = _loginState

    fun login() {
        _loginState.value = true
    }
}
```

- UI

```kotlIN
import androidx.compose.foundation.layout.Column
import androidx.compose.material3.Button
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.lifecycle.compose.collectAsStateWithLifecycle

@Composable
fun LoginScreen(viewModel: LoginViewModel) {
    val isLoggedIn = viewModel.loginState.collectAsStateWithLifecycle()

    Column {
        if (isLoggedIn.value) {
            Text("로그인 성공")
        } else {
            Text("로그인 필요")
        }

        Button(onClick = {
            viewModel.login()
        }) {
            Text("로그인")
        }
    }
}
```
