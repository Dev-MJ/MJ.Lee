## UICollectionViewFlowLayout
* 각 section에 대해 header view, footer view를 선택적으로 이용하여 **item들을 grid 형태로 조직화 하는 layout 객체**이다.
* collectionview의 item들은, 한 row 또는 column에서 다음 row 또는 column으로 이어진다.(각 row 또는 column은 적당한 크기만큼의 cell로 구성된다)
* cell들의 size는 같을 수도, 다를 수도 있다.
* **flow layout은 collectionview의 delegate object와 함께 각 section의 item, header, footer의 size와 grid를 결정한다.**
* **delegate object는 반드시 UICollectionViewDelegateFlowLayout protocol을 준수해야 한다.**
* delegate를 사용하면 레이아웃을 동적으로 조정할 수 있는데, 예를 들어 delegate object를 사용하여 grid item의 size를 다르게 지정할 수 있다. <br/> 만약 delegate를 제공하지 않는다면, flow layout은 UICollectionViewFlowLayout클래스의 default value를 사용한다.
* flow layout은, **한 방향으로는 고정된 거리를 사용**하고 **다른 방향으로는 스크롤 가능한 거리를 사용**하여 content를 배치한다. <br/>
예를 들어 세로로 스크롤하는 grid에서, content width는 해당 collectionview의 너비로 제한되지만 content height는 section 및 item 수와 동적으로 조정된다.
* flow layout은 기본적으로 vertical scroll이지만, scrollDirection 속성을 사용하여 방향을 변경할 수 있다.
* flow layout의 각 section에서는 각자의 header와 footer를 가질 수 있다.
* **footer 또는 header를 설정하기 위해서는, 이들의 size를 zero로 해서는 안된다.** (***적절한 delegate method를 구현하거나, headerReferenceSize 또는 footerReferenceSize property를 할당해야 한다.***) <br/>
만약 header 또는 footer의 **size가 0이라면, collectionview에 add되지 않는다.**



## UICollectionViewDelegateFlowLayout
* Grid-based 레이아웃을 구현하는 **UICollectionViewFlowLayout object를 조정하는 method 들을 정의**하는 protocol이다.
   * object들은 다음과 같다.
      ``` swift
      var scrollDirection: UICollectionViewScrollDirection
      var minimumLineSpacing: CGFloat
      var minimumInteritemSpacing:CGFloat
      var itemSize: CGSize
      var estimatedItemSize: CGSize
      var sectionInset: UIEdgeInsets
      var headerReferenceSize: CGSize
      var footerReferenceSize: CGSize
      var sectionHeaderPinToVisibleBounds: Bool
      var sectionFooterPinToVisibleBounds: Bool
      enum UICollectionViewScrollDirection
      let UICollectionViewFlowLayoutAutomaticSize: CGSize
      var sectionInsetReference: UICollectionViewFlowLayoutSectionInsetReference
      ```
* **이 protocol의 모든 method은 optional이며, 특정 method를 구현하지 않으면 flow layout delegate는 spacing 정보에 대해 default value를 사용한다.**
* **flow layout 객체는, collectionview의 delegate object가 이 protocol을 채택할 것이라 기대하기 때문에, collectionview의 delegate property에 이 protocol을 구현한 객체를 할당해야 한다.**



## Core Layout Process
* layout 정보가 필요할 때, collectionview는 layout object에게 다음과 같은 process를 통해 특정 method들을 call하여 layout 정보를 얻는다.

![core_layout_process](https://koenig-media.raywenderlich.com/uploads/2015/05/layout-lifecycle.png)

* 따라서 custom layout을 만들 경우, 아래의 method들을 반드시 구현해야 한다.
   * `collectionVIewContentSize`
      * collectionView의 content size를 return 한다. collectionView는 이 정보를 내부적으로 이용하여 scrollView의 content size를 설정한다.
   * `prepare()`
      * layout 동작이 발생하기 직전에 호출된다. collectionView의 size와 item들의 position을 결정하는데 필요한 각종 계산들을 준비하고 수행할 수 있는 기회(?)이다.
   * `layoutAttributesForElements(in:)`
      * 주어진 rectangle 내에 있는 모든 item들에 대한 layout attributes를 return한다. UICollectionViewLayoutAttributes 타입의 array로 collectionView에 attributes를 return 한다.
   * `layoutAttributesForItem(at:)`
      * demand layout 정보를 collectionView에 제공한다. 이 method를 override하고, 요청된 indexPath에서 item의 layout attributes를 return해야 한다.
