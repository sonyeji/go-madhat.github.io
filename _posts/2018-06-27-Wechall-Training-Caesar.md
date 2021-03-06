---
layout: post
title: ! "[WeChall] Training: Crypto - Caesar I, Transposition I WriteUp"
tags :
  - Write-up
  - WeChall
  - GaeKo
---

안녕하세요. 암호를 공부하는 Gaeko입니다. 
암호 공부를 위해 WeChall crypto문제를 풀기 시작하였습니다. 그래서 이번 포스팅을 푼 문제에 대한 writeup을 올리기로 하였습니다. 

## Training: Crypto - Caesar I

**[문제]**

![문제](https://t1.daumcdn.net/cfile/tistory/99FFBE4E5B327C7408)

**[풀이]**
 
시저 암호에 관한 문제인거 같다. 처음 들어보는 나는 시저암호가 무엇인지 찾아보았다.

**시저 암호(Ceasar cipher)란 암호학에서 다루는 간단한 치환암호의 일종이다. 암호화하고자 하는 내용을 알파벳별로 일정한 거리만큼 밀어서 다른 알파벳으로 치환하는 방식이다. -wiki-**
 
처음엔 뭣도 모르고 손으로 써가면서 찾아보았는데 코드로 돌리면 된다는 말을 듣고 코드를 짜보았다.

```python
## WeChall cryto-Ceasar 1 
 
 
# a : 암호화된 문장
a = "NBY KOCWE VLIQH ZIR DOGJM IPYL NBY FUTS XIA IZ WUYMUL UHX SIOL OHCKOY MIFONCIH CM JAYLJMIYYFFG"
 
for j in range(25): # j is 0 ~ 25
        print 'j : %d' % j
        c = '' # result 
        for i in range(len(a)):
                if(a[i] != " "):
                        k = (ord(a[i])+j-65)%26 + 65 # 65 = chr('A')
                        c += chr(k)
                else:
                        c += ' '
        print c
```


위 코드는 암호화된 문장에 대해서 알파벳별로 j만큼 밀어낸 결과의 알파벳으로 출력해준다.

(암호화문이 대문자만 존재하는 경우에 대해서만 생각하였다.) 

변수 a는 암호화된 문장이고 변수 c에는 a의 각 문자열에 대해서 공백이 아닌 문자열만 j만큼 밀어낸 결과를 저장한다. 
변수 k는 변환되는 아스키코드값을 저장하는 역할을 한다. 

위 코드를 돌린 결과는 다음과 같다.

![output](https://t1.daumcdn.net/cfile/tistory/99FE26485B3280830C)

j=6일 때가 평문임을 알 수 있다. 

즉, 평문에서 알파벳을 뒤로 6칸 밀은 알파벳으로 치환했음을 알 수 있었다.

![site](https://t1.daumcdn.net/cfile/tistory/99B4C4455B3281590D)

Clear! 



다른 사람들이 짠 코드를 보려고 구글링해보니 시저암호를 복호화하는 함수를 정의하여 사용하는 걸 보고 나도 해보았다. ㅎㅎ 


```python
# Decryption Function of Ceasar crypto
 
def ceasar(ciphertext, key):
        plaintext = ''
        for i in ciphertext:
                if(i.islower()): # i is small letter
                        plaintext += chr((ord(i) + key - ord('a')) % 26 + ord('a'))
                elif(i.isupper()): # i is capital letter
                        plaintext += chr((ord(i) + key - ord('A')) % 26 + ord('A'))
                else: # i is blank
                        plaintext += ' '
        return plaintext
 
 
# Test
a = "NBY KOCWE VLIQH ZIR DOGJM IPYL NBY FUTS XIA IZ WUYMUL UHX SIOL OHCKOY MIFONCIH CM JAYLJMIYYFFG"
print ceasar(a, 6)
```

- ord(i) : 문자 i에 대해 아스키코드 값을 반환 
- chr(n) : 아스키코드 값 n에 대해서 문자를 반환 
- i.islower() : 문자열 i 에 대해서 모두 소문자이면 true 
- i.isupper() : 문자열 i 에 대해서 모두 대문자이면 true 

cf. [문자열 관련 함수](https://kimdoky.github.io/python/2017/10/06/library-book-chap1-1.html)


## Training: Crypto - Transposition I

**[문제]**

![문제](https://t1.daumcdn.net/cfile/tistory/9926A94F5B3292661F)

암호문 
**oWdnreuf.lY uoc nar ae dht eemssga eaw yebttrew eh nht eelttre sra enic roertco drre . Ihtni koy uowlu dilekt  oes eoyrup sawsro don:wp pegrfdmooi.r**


**[풀이]**

전치 암호에 대한 문제인가보다. 역시나 잘 몰라서 찾아보았다. 

전치 암호(Tranposition cipher)란 평문의 순서를 재배치하여 암호화하는 방법이다. 

만약 key가 "54231"이고 문자열의 위치를 [이동전 -> 이동후]라고 표현한다면 다음과 같이 이동한다. 
[1->5]
[2->4]
[3->2]
[4->3]
[5->1]
그럼 복호화하는 경우에는 거꾸로 생각해주면 되겠다.

이 문제에서는 앞부분 oWdnreuf.lY 를 통해 키가 "21"임을 유추할 수 있었다.

```python
## wehChall Training: Crypto - Transposition I (Crypto, Training) 
 
def transposition(ciphertext, key):
        plaintext = ''
        key = list(key)
        numOfBlock = len(ciphertext) / len(key)  # num of block -1
        for block in range(numOfBlock): # block: 0 ~ num of block-1
                for i in key:
                        k = int(i) - 1 + block*len(key)
                        if(k < len(ciphertext)):
                                plaintext += ciphertext[k]
        return plaintext
 
# a is ciphertext and key is "21"
a = "oWdnreuf.lY uoc nar ae dht eemssga eaw yebttrew eh nht eelttre sra enic roertco drre . Ihtni koy uowlu dilekt  oes eoyrup sawsro don:wp pegrfdmooi.r"
print "> ciphertext\n" + a
print "> plaintext\n" + transposition(a, "21")
```

위 코드를 실행해보면 다음과 같다.

![output](https://t1.daumcdn.net/cfile/tistory/993EBD4B5B329FE82B)

즉, 암호문을 복호화하면 
**Wonderful. You can read the message way better when the letters are in correct order. I think you would like to see your password now: peprgdfomior.**
임을 알 수 있다!

![site](https://t1.daumcdn.net/cfile/tistory/995BE2505B32A04228)

Clear!  