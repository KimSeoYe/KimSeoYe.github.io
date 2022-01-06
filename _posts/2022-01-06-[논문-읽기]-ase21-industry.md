---
layout: post
title:  "[논문 읽기] ase21-industry"
date:   2022-01-06 15:00:00
author: KimSeoYe
categories: TIL
tags:   TIL
cover:  "/assets/headers/hgu1.jpeg"
---

# [논문 읽기] Improving Configurability of Unit-level Continuous Fuzzing : An Industrial Case Study with SAP HANA

> 논문 전문 : https://hongshin.github.io/pubs/ase21-industry.pdf 


## Summary

- customized mutation operator들을 효율적으로 이용하기 위한 5개의 새로운 mutation scheduling strategies 제안
- **3개의 새로운 seed corpus selection strategies 제안**
- 결과 : Fuzzing framework을 target component에 맞게 더 명확하게 configuration할 수 있도록 함으로써 Fuzzing의 성능 향상


## IV. Configurability for Seed Corpus Selection

### Motivation and Proposed Technique

- Initial seed corpus : Fuzzing의 search scope를 결정하는 key factor.
- 어떤 컴포넌트에서 코드의 특정 부분에만 수정이 있을 때 (local change), Fuzzer가 해당 변화만 빠르게 체크하도록 하기를 원한다면? => General-purpose seed corpus 대신, **수정된 코드와 관련된(relevant) seed corpus**를 사용하면 좋을 것이다.

#### Proposed Technique

1. Resulting corpus와 Coverage information을 함께 저장
2. 바뀐 코드 부분을 커버하는 test input들의 subset을 모아 change-specific seed corpus를 만든다.

#### Overal Workflow of The Proposed Technique

1. General mode : general-purpose seed corpus 사용
    - General mode fuzzing이 끝나면 생성된 corpus가 corpus database로 전달됨. (* resulting corpus : 적어도 하나의 unique한 coverage item을 커버한 인풋들의 subset)    
    - **_per-function seed corpus_** : 각 test input에 대해 실행된 function들을 idnetify => 해당 function을 커버하는 test input들의 set을 per-function seed corpus로 corpus database에 저장. 

2. Local mode : Target component에 Local code change가 발생했을 때, 수정된 버전의 failure를 빠르게 체크하고자 할 경우 사용.
   - Change analysis module : 코드가 수정된 모든 function을 identify. (`diff` 사용)
   - Corpus selection module : 