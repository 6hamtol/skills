# Smart Brevity Before/After 예시

---

## 예시 1 — 한국어 Slack 확장 (키워드 → 공지)

**입력 (키워드):**
> 신규 장바구니 자동저장 기능 출시, 5월 26일, 베타 50명, 피드백 폼 있음, 담당자 육승재

**❌ 기본 출력 (Bad):**
> 📢 **신규 기능 출시 안내: 장바구니 자동저장**
> 안녕하세요! 5월 26일(월)부터 장바구니 자동저장 기능이 베타 오픈됩니다. 이번 베타에는 사전 선정된 50명이 참여하며...

**✅ Smart Brevity 출력:**
```
장바구니 자동저장 베타 오픈
5월 26일부터 장바구니에 담은 상품이 재접속해도 유지됩니다.
*왜 중요*: 이탈 후 재접속 시 처음부터 다시 담는 불편이 사라집니다.
• 베타 대상: 사전 선정 50명
• 피드백 폼 링크 (출시일 공유 예정)
• 문의: 육승재
```

**왜 바뀌었나:** "안녕하세요" 제거, 이모지 제거, "왜 중요한가" 추가, 헤드라인 12자로 압축.

---

## 예시 2 — 한국어 이메일 리라이팅

**입력 (긴 초안):**
> 안녕하세요, 이번에 저희 팀에서는 유튜브 영상에서 재료를 추출해 Kurly 장바구니에 자동으로 담아주는 PoC 시스템을 개발하였습니다. 이 시스템은 사용자가 유튜브 요리 영상 URL을 입력하면 Claude AI를 이용해 재료를 추출하고, Kurly 검색 API를 통해 관련 상품을 찾아 장바구니에 담아주는 방식으로 동작합니다. 현재까지 테스트 결과 재료 추출 정확도는 약 85% 수준이며 카트 연동 성공률은 92%입니다. 다음 단계로는 UI 개선과 사용자 테스트를 진행할 예정입니다. 팀원 여러분의 많은 관심과 피드백을 부탁드립니다.

**✅ Smart Brevity 출력:**
```
제목: 유튜브 영상 → Kurly 장바구니 PoC 결과

YouTube URL 하나로 Kurly 장바구니가 자동으로 채워집니다.

**왜 중요한가**
요리 콘텐츠 시청에서 구매까지의 마찰을 제거하는 PoC입니다.
쿠팡 아닌 Kurly로 직접 연결되는 검증이 처음으로 됐습니다.

**수치**
• 재료 추출 정확도: 85%
• 카트 연동 성공률: 92%

**다음 단계**
• UI 개선
• 사용자 테스트 진행

피드백 환영합니다.
```

**왜 바뀌었나:** "안녕하세요" 제거, "이번에 저희는" 제거, 결론 먼저, "왜 중요한가" 추가, 수치는 bullet으로 정렬.

---

## 예시 3 — 한국어 기획서 도입부 리라이팅

**입력:**
> 이번 제안서는 저희가 개발하고 있는 레시피 기반 자동 장바구니 서비스를 정식 서비스로 전환하는 것에 대한 내용을 담고 있습니다. 현재 PoC 결과를 바탕으로 사용자 경험 개선, 기술 안정성 확보, 비즈니스 가치 검증을 위한 다음 단계 투자를 요청드리고자 합니다.

**✅ Smart Brevity 출력:**
```
# 레시피 장바구니 PoC → 정식 서비스 전환 제안

PoC 검증 완료. 이제 실서비스로 만들 차례입니다.

## 왜 중요한가
요리 콘텐츠 → 구매 전환 경로는 현재 끊겨 있습니다.
이 서비스가 연결하면 Kurly만의 콘텐츠 커머스 차별화가 생깁니다.

## 현황
• 재료 추출 정확도: 85%
• 카트 연동 성공률: 92%
• PoC 검증 기간: 3주

## 다음 단계
• 정식 서비스 개발 투자 승인
• UI/UX 개선 및 사용자 테스트

## 결론
지금이 전환 시점입니다.
```

**왜 바뀌었나:** "이번 제안서는 ~담고 있습니다" 제거, 결론 헤드라인으로, "왜 중요한가"를 구체적으로.

---

## 예시 4 — English Slack (expansion)

**Input (keywords):**
> Cart auto-save feature launch, May 26, 50 beta users, feedback form, owner Seungjae

**✅ Smart Brevity output:**
```
Cart auto-save goes live May 26
Items stay in your cart after you leave and come back.
*Why it matters*: No more rebuilding your cart from scratch after a session ends.
• Beta: 50 users (invite-only)
• Feedback form link (shared on launch day)
• Questions: @Seungjae
```

---

## 예시 5 — English email (rewrite)

**Input (long draft):**
> Hello everyone, I wanted to share that our team has been working on a proof-of-concept system that extracts ingredients from YouTube cooking videos and automatically adds them to the Kurly shopping cart. The system works by having the user input a YouTube URL, then Claude AI extracts the ingredients, and the Kurly Search API finds matching products. Our tests show an ingredient extraction accuracy of approximately 85% and a cart integration success rate of 92%. Next steps will include UI improvements and user testing. We appreciate your interest and feedback.

**✅ Smart Brevity output:**
```
Subject: YouTube → Kurly cart PoC results

One YouTube cooking URL fills your Kurly cart automatically.

**Why it matters**
This closes the gap between recipe content and purchase — entirely within Kurly.

**The numbers**
• **Ingredient accuracy:** 85%
• **Cart success rate:** 92%

**What's next**
• UI improvements
• User testing

Feedback welcome.
```

---

## 예시 6 — English proposal intro (rewrite)

**Input:**
> This proposal outlines our recommendation to transition the recipe-based automatic cart service from its current proof-of-concept phase to a full production service. Based on our PoC results, we are requesting investment in the next phase of development to improve user experience, ensure technical stability, and validate business value.

**✅ Smart Brevity output:**
```
# PoC validated. Time to ship the recipe cart.

The data says go. This proposal asks for the investment to do it.

## Why it matters
Content-to-cart conversion is broken for every competitor.
Kurly can own this space — but only if we move now.

## The numbers
• Ingredient accuracy: 85%
• Cart success rate: 92%
• PoC timeline: 3 weeks

## What's next
• Approve production development investment
• Begin UX improvements and user testing

## Bottom line
Ship it.
```
