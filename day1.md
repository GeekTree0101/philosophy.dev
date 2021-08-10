# 설계는 균형의 예술(Software design is the art of balance)
오후 늦은 시간 나는 동료의 코드를 리뷰하는 과정에서 어떤 객체를 보았다.
그 객체의 내부로직에서는 기획 및 UI/UX측면에서 정의된 스펙을 처리함과 동시에 특수한 Entity를 저장 및 불러오는 역할 또한 하고 있었다.
이것은 Presentation역할을 하는 객체이면서도 특수한 Entity를 저장 및 불러오는 역할을 하는 Repository역할을 하는 객체역할을 하고 있는 것이다. 
그렇다고 Presentation역할과 Repository역할을 각각 객체로 분리하자니 Repository로 나누더라도 사실 다른 객체의 dependency로 다뤄질 일은 앞으로도 없는게 사실이고 또한 오히려 관리포인트만 늘어나는 것이 아닌가.
만약 다른 영역에서 특수한 Entity의 Repository역할이 필요한 객체가 생기면 언제든 그 때 고수준 객체로 분리하는건 어렵지 않으니 나는 이 객체에 대해서 approve를 했다. 

In the afternoon, i saw an special object while a pull-request review.
It's internal logic was responsible for processing defined specifications in terms of planning and UI/UX, also storing and recalling special entity(or a.k.a model).
To be more specific, it acts not only presentation role but also repository role.
However, separating the presentation role & repository role into objects, it is true that even if divided into repository, it will not be handled as the dependency of other objects in the future, and rather, only the development management points will be increased.
If there is an object in other areas that needs a special entity repository role, it is not difficult to separate it into a high-level layer object at anytime!, so I have approved this object.

```swift
protocol StarServiceProtocol { 

  func append(_ star: Star)
  func all() -> [Star]
}

struct StarService: StarServiceProtocol {

  let persistanceLayer: PersistanceLayerProtocol

  func append(_ star: Star) {

    var stars = self.persistanceLayer.load(\.star)
    if stars.contained(star) {
      return
    }

    stars.insert(star, at: 0)
    self.persistanceLayer.save(\.star, Array(stars.prefix(4)))
  }


  func all() -> [Star] {

    return self.persistanceLayer.load(\.star)
  }
}
```

### In the future
```swift

// presentation

protocol StarPresentationProtocol {

  func append(_ star: Star)
  func all() -> [Star]
}

struct StarPresentation: StarPresentationProtocol {

  let starRepository: StarRepositoryProtocol
  var appendIndexAtZero: Bool = true
  var limitBuffer: Int = 4

  func append(_ star: Star) {
    var stars = starRepository.all()

    if stars.contained(star) {
      return
    }

    if self.appendIndexAtZero {
      stars.insert(star, at: 0)
    } else {
      stars.append(star)
    }
    starRepository.save(Array(stars.prefix(self.limitBuffer)))
  }

  func all() -> [Star] {
    return starRepository.all()
  }

}

// repository

protocol StarRepositoryProtocol {

  func append(_ star: Star, at index: Int)
  func all() -> [Star]
  func save(_ stars: [Star])
}

struct StarRepository {

  let persistanceLayer: PersistanceLayerProtocol

  func append(_ star: Star, at index: Int) {

    var stars = self.persistanceLayer.load(\.star)
    stars.insert(at: index, star)
    self.persistanceLayer.save(\.star, Array(stars))
  }

  func all() -> [Star] {

    return self.persistanceLayer.load(\.star)
  }

  func save(_ stars: [Star]) {

    self.persistanceLayer.save(\.star, stars)
  }
}

```