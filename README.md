# 🖼 ImageSlideshow

**Customizable Swift image slideshow with circular scrolling, timer and full screen viewer**

[![CI](https://github.com/Caplord/ImageSlideshow/actions/workflows/ci.yml/badge.svg)](https://github.com/Caplord/ImageSlideshow/actions/workflows/ci.yml)
[![Carthage Compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)



![](https://dzwonsemrish7.cloudfront.net/items/2R06283n040V3P3p0i42/ezgif.com-optimize.gif)

## 📱 Example

To run the example project, clone the repo, and run `xcodegen generate` from the Example directory first.

## 🔧 Installation

### Swift Package Manager
ImageSlideshow is available through the [Swift Package Manager](https://www.swift.org/package-manager/). To install
it, add the following dependency to your `Package.swift`, or add it via Xcode's *File > Add Package Dependencies…*:

```swift
.package(url: "https://github.com/Caplord/ImageSlideshow.git", from: "1.10.0")
```

Then add the `ImageSlideshow` product to your target, along with whichever input-source products you need (`ImageSlideshowAlamofire`, `ImageSlideshowSDWebImage`, `ImageSlideshowKingfisher`).

### Carthage
To integrate ImageSlideshow into your Xcode project using Carthage, specify it in your Cartfile:

```ruby
github "Caplord/ImageSlideshow" ~> 1.10.0
```

Carthage does not include InputSources for external providers (due to dependency on those providers) so you need to grab the one you need from `ImageSlideshow/Classes/InputSources` manually.

### Manually
You can also grab the whole `ImageSlideshow` directory and copy it to your project. Be sure to remove those external Input Sources you don't need.

## 🔨 How to use

Add ImageSlideshow view to your view hiearchy either in Interface Builder or in code.

### Loading images

Set images by using ```setImageInputs``` method on ```ImageSlideshow``` instance with an array of *InputSource*s. By default you can use ```ImageSource``` which takes ```UIImage``` or few other *InputSource*s for most popular networking libraries. You can also create your own input source by implementing ```InputSource``` protocol.

| Library                                                       | InputSource name | SPM product                |
| ------------------------------------------------------------- |:----------------:| ---------------------------:|
| [AlamofireImage](https://github.com/Alamofire/AlamofireImage) | AlamofireSource  | `ImageSlideshowAlamofire`  |
| [SDWebImage](https://github.com/rs/SDWebImage)                | SDWebImageSource | `ImageSlideshowSDWebImage` |
| [Kingfisher](https://github.com/onevcat/Kingfisher)           | KingfisherSource | `ImageSlideshowKingfisher` |


```swift
slideshow.setImageInputs([
  ImageSource(image: UIImage(named: "myImage"))!,
  ImageSource(image: UIImage(named: "myImage2"))!,
  AlamofireSource(urlString: "https://images.unsplash.com/photo-1432679963831-2dab49187847?w=1080"),
  KingfisherSource(urlString: "https://images.unsplash.com/photo-1432679963831-2dab49187847?w=1080")
])
```

### Configuration

Behaviour is configurable by those properties:

- ```slideshowInterval``` - slideshow interval in seconds (default `0` – disabled)
- ```zoomEnabled``` - enables zooming (default `false`)
- ```circular``` - enables circular scrolling (default `true`)
- ```activityIndicator``` – allows to set custom activity indicator, see *Activity indicator* section
- ```pageIndicator``` – allows to set custom page indicator, see *Page indicator* section; assign `nil` to hide page indicator
- ```pageIndicatorPosition``` - configures position of the page indicator
- ```contentScaleMode``` - configures the scaling (default `ScaleAspectFit`)
- ```draggingEnabled``` - enables dragging (default `true`)
- ```currentPageChanged``` - closure called on page change
- ```willBeginDragging``` - closure called on scrollViewWillBeginDragging
- ```didEndDecelerating``` - closure called on scrollViewDidEndDecelerating
- ```preload``` - image preloading configuration (default `all` preloading, also `fixed`)

### Page Indicator

Page indicator can be customized using the `pageIndicator` property on ImageSlideshow. By defualt, a plain UIPageControl is used. If needed, page control can be customized:

```swift
let pageIndicator = UIPageControl()
pageIndicator.currentPageIndicatorTintColor = UIColor.lightGray
pageIndicator.pageIndicatorTintColor = UIColor.black
slideshow.pageIndicator = pageIndicator
```

Also, a simple label page indicator that shows pages in style "5/21" (fifth page from twenty one) is provided:

```swift
slideshow.pageIndicator = LabelPageIndicator()
```

You can also use your own page indicator by adopting the `PageIndicatorView` protocol.

Position of the page indicator can be configured by assigning a `PageIndicatorPosition` value to the `pageIndicatorPosition` property on ImageSlideshow. You may specify the horizontal and vertical positioning separately.

**Horizontal** positioning options are: `.left(padding: Int)`, `.center`, `.right(padding: Int)`

**Vertical** positioning options are: `.top`, `.bottom`, `.under`, `customTop(padding: Int)`, `customBottom(padding: Int)`, `customUnder(padding: Int)`

Example:
```swift
slideshow.pageIndicatorPosition = PageIndicatorPosition(horizontal: .left(padding: 20), vertical: .bottom)
```


### Activity Indicator

By default activity indicator is not shown, but you can enable it by setting `DefaultActivityIndicator` instance to Image Slideshow:

```swift
slideshow.activityIndicator = DefaultActivityIndicator()
```

You can customize style and color of the indicator:

```swift
slideshow.activityIndicator = DefaultActivityIndicator(style: .white, color: nil)
```

There's also an option to use your own activity indicator. You just need to implement `ActivityIndicatorView` and `ActivityIndicatorFactory` protocols. See `ActivityIndicator.swift` for more information.

### Full Screen view

There is also a possibility to open full-screen image view using attached `FullScreenSlideshowViewController`. The simplest way is to call:

```swift
override func viewDidLoad() {
  let gestureRecognizer = UITapGestureRecognizer(target: self, action: #selector(ViewController.didTap))
  slideshow.addGestureRecognizer(gestureRecognizer)
}

func didTap() {
  slideshow.presentFullScreenController(from: self)
}
```

`FullScreenSlideshowViewController` can also be instantiated and configured manually if more advanced behavior is needed.

## 👤 Author

Petr Zvoníček

## 📄 License

ImageSlideshow is available under the MIT license. See the LICENSE file for more info.

## 👀 References

Inspired by projects:
- https://github.com/gonzalezreal/Vertigo
- https://github.com/kimar/KIImagePager
