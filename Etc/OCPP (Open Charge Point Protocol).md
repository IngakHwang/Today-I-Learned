# OCPP (Open Charge Point Protocol)

> EV 충전 표전 프로토콜
>
> 공급업체에 관계없이 모든 중앙 시스템을 모든 충전 지점과 연결



OCPP는 충전기/충전소와 충전사업자가 운영하는 중앙시스템간의 통신 규약으로 인증, 충전 및 충전기 관리 등

전기차 충전사업자가 서비스를 운영하는데 필요한 데이터를 표준화한 Protocol



EV충전소를 위한 개방형 네트워킹 ex) 핸드폰을 구입할 때 여러 셀루러 네트워크 중에서 선택 할 수 있다.



유럽 OCA (Open Charge Alliance)에 의해 OCPP1.5를 발표하면서 많은 사업자의 주목, 채택을 받기 시작했다.

국내 전기차 충전기에도 OCPP 적용이 점차 확대되고 있는 추세이다.



## 전기차 충전 표준

전기자동차는 차량에 따라 충전 커플러(커넥터) 타입이 다르다.

이는 각 차량에 적용된 충전 표준이 다르기 떄문이다.



충전기는 크게 완속 충전기, 급속 충전기로 나뉜다.



완속은 충전속도가 천천히 진행된다는 뜻으로, 국내에서는 대부분의 완속 충전기는 AC 5핀 규격을 사용한다.

AC는 교류전기를 사용한다는 뜻으로, 가정용 저전압(220V)을 사용하고 평균 충전속도는 5~7시간이다.



급속은 고전압 직류전기를 통해 빠르게 충전하는 방식으로 DC콤보, 차 데모, AC 3상 그리고 테슬라 슈퍼차저 4가지의 충전타입이 있다.

DC콤보는 콤보 타입 1로 불리기도 하며, AC 5핀과 DC 2핀이 하나의 커넥터에 구성되어 있다.

국내 규격도 DC 콤보로 제정되어서, 볼트, 코나, 아이오닉, 니로, 쏘울도 해당 표준을 사용하여 충전한다.



## 국내 충전 인프라

국내 전기차 충전사업자는 공공기관인 환경부 산하의 한국환경공단, 포스코ICT, 한충전 등 8개의 민간사업자(대영 채비대영채비, 파워큐브, 에버론에버온, 지엔텔, 제주 전기차 서비스, KT)가 운영하고 있다.



전기차 충전 사업자는 자사의 충전서비스를 이용하는 전기 자동차 운전자에게 회원 가입 및 결제카드를 발급한다.

이를 통해 최초 충전 시도 시 충전기를 통해 회원카드를 통해 회원임을 인증하고, 충전요금을 정산받게 된다.

그렇다면, 운전자가 방문하는 충전소마다 각각의 충전사업자용 회원카드가 별도로 있어야 할까?



이는 로밍(Roaming) 이라는 회원, 정산정보 통합이라는 기술을 통해 모든 충전사업자는 자기 회원이 아니더라도 충전을 받을 수 있게끔 상호 간의 회원정보, 충전정보를 교환하게 된다.

이렇게 되면 운전자는 언제 어디서 충전하더라도 자신이 가입한 충전 서비스를 통해 통합 정산을 받게 된다.

국내에서는 환경부가 각 충전사업자 간에는 회원정보 및 충전정보를 공유하여 사업자 간에 정산할 수 있는 전산통합이 이뤄져 있는 만큼 국내 어디서라도 편리한 충전 서비스를 받을 수 있게 된다.



## 충전기와 중앙충전관리시스템(CPMS) 간의 개방형 프로토콜 OCPP

충전기와 중앙충전관리시스템 간의 개방형 프로토콜인 OCPP (Open Charge Point Protocol)은 무엇일까?

OCPP 는 충전기(충전소)와 충전사업자가 운영하는 중앙시스템 간의 통신규격을 표준화한 것으로,

인증, 충전 및 충전기 관리 등 전기차 충전사업자가 서비스를 운영하는 데 필요한 데이터를 표준화하였다.



OCPP 1.5 는 충전기 운영에 필요한 충전기와 중앙시스템 간의 핵심 코어 기능을 위주로 정의되었으며,

OCPP 1.6 에서는 충전기 운영강화를 위한 트리거 메시지 추가 및 전력수요예측 기반의 스마트충전이 추가되었다.

OCPP 2.0 은 이전 버전에서 다루지 않았던 인증서 기반의 통신 및 스마트충전 기능 강화 그리고 IEC/ISO 15118과 연계되는 Plug&Charge를 다룬다.



## 자동차 사이버보안 관리시스템 평가 (CSMS)

> Cyber Security Management System
>
> OEM의 사이버보안 관리 시스템에 대한 심사

OEM의 사이버보안 관리 시스템에 대한 심사

전문가는 조직의 프로세스가 제품 생애 주기 전체에 걸쳐 적합한 사이버보안 프레임워크를 제공하는지, 

UNECE 사이버보안 차량 규제의 CSMS 요구 사항을 모두 충족하는지를 평가



**UNECE 사이버보안 규제 관련, 제조업체에 대한 요구사항**

제조업체는 인증된 CSMS를 유지해야 하며, 최소 3년마다 재평가 및 갱신을 해야 한다.

안전성, 보안성을 갖춘 제품을 생산하기 위해 개발, 생산, 생산 후 과정 전반에 걸쳐 조직이 적절한 보안 조치를 취하도록 보장



CSMS에 대한 증거를 제공하지 못하면 형식승인을 받을 수 없고, EU에서 차량, 부품, 소프트웨어를 판매할 수 없다.



---

참고 사이트

엔텔스 : https://www.ntels.com/insight_201904_04/

OCA : https://www.openchargealliance.org/protocols/

네이버 블로그 - CSMS : https://m.blog.naver.com/tuv-sud/222096624030

생활법령정보 : https://www.easylaw.go.kr/CSP/CnpClsMain.laf?popMenu=ov&csmSeq=1593&ccfNo=2&cciNo=2&cnpClsNo=1