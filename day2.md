# 모든건 하나의 점에서 부터 시작된다
동료가 물었다. VoIP와 webRTC의 차이점은 무엇인가요? Agora는 AgoraRTC를 제공하는데 그러면 webRTC를 채택하고 있는 걸까요?
사전적 의미로만 보았을 때 둘은 비숫해보이기도 하면서도 한편으론 다른것 같아보이지만 결론부터 말하자면 AgoraRTC는 webRTC를 온전히 채택한 것은 아니며 AgoraRTC중 Agora의 VoIP는 통상적으로 알고 있는 VoIP의 저수준 서비스다.

#### VoIP 구조 예시
<img src="https://github.com/GeekTree0101/philosophy.dev/blob/main/res/voip.png" />

그렇다면 webRTC는 무엇인가? 

#### webRTC 구조 예시
<img src="https://github.com/GeekTree0101/philosophy.dev/blob/main/res/webrtc.png" />

webRTC내부엔 voice engine이 있고 두가지의 코덱(codec)이 있는데 그 중에서 Session Initiation
Protocol(SIP)를 혹시 저수준 레이어에서 지원하고 있지않을까?

결론적으론 iLBC 코덱은 SIP와 H.323. 을 지원하는 것이기에 webRTC에서 voice engine이 VoIP를 기반으로 만들었다 볼 수 있다.
```
The internet Low Bitrate Codec (iLBC) is a standard, high-complexity speech codec suitable for robust
voice communication over IP. The iLBC has built-in error correction functionality that helps the codec
perform in networks with high-packet loss. This codec is supported on both Session Initiation
Protocol (SIP) and H.323.
```

이처럼 단순 사전적의미로만 파악하기 힘든 문제나 상관관계를 파악하기 위해선 하나의 점에서 부터 시작된다는 생각을 가지고 접근하면 풀릴 때도 있다는 점. 이것이 오늘의 깨달음이다.
