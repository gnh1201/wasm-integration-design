Note: This document is written in Korean. If anyone has any questions, please email support@exts.kr

## 문제 정의
마이크로소프트의 엑셀(Excel)에서는 매크로(Macro)라는 개념이 많이 사용되지만 VBScript 등 특정 언어에 제한되어 있고, 구현된 코드가 사실상 다른 플랫폼에서는 재활용할 수 없는 문제를 가지고 있다.

게다가 매크로가 많은 시스템 작업을 허용하기 때문에 믿을 수 없는 코드(aka. Untrustable Code)에 대한 통제력을 가지기가 쉽지 않다.

엑셀에서 매크로를 구현할 수 있는 언어 종류의 제약을 풀고, Untrustable Code를 통제할 수 있으며, 엑셀 문서를 위해 구현된 함수를 다른 플랫폼을 위해서 사용하고자 하는 요구가 있다. 이것을 해결할 방안으로 WebAssembly는 과연 적합할까?




