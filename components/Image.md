A react component for displaying different types of images, including network images, static resources, temporary local images, and images from local disk, such as the camera roll.

Example usage:

```html
renderImages: function() {
  return (
    <View>
      <Image
        style={styles.icon}
        source={require('image!myIcon')}
      />
      <Image
        style={styles.logo}
        source={{uri: 'http://facebook.github.io/react/img/logo_og.png'}}
      />
    </View>
  );
},
```

## Props 

**accessibilityLabel** string 

accessibilityLabel - Custom string to display for accessibility.

**accessible** bool 

accessible - Whether this element should be revealed as an accessible element.

**capInsets** {top: number, left: number, bottom: number, right: number} 

capInsets - When the image is resized, the corners of the size specified by capInsets will stay a fixed size, but the center content and borders of the image will be stretched. This is useful for creating resizable rounded buttons, shadows, and other resizable assets. More info:

[https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIImage_Class/index.html#//apple_ref/occ/instm/UIImage/resizableImageWithCapInsets:](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIImage_Class/index.html#//apple_ref/occ/instm/UIImage/resizableImageWithCapInsets:)

**source** {uri: string} 

**style** style 

├─[**Flexbox...**](http://facebook.github.io/react-native/docs/flexbox.html#proptypes)
├─**backgroundColor** string
├─**borderColor** string
├─**borderRadius** number
├─**borderWidth** number
├─**opacity** number
├─**resizeMode** Object.keys(ImageResizeMode)
├─**tintColor** string
└─**testID** string 

testID - A unique identifier for this element to be used in UI Automation testing scripts.

Displaying images is a fascinating subject, React Native uses some cool tricks to make it a better experience.

## No Automatic Sizing

If you don't give a size to an image, the browser is going to render a 0x0 element, download the image, and then render the image based with the correct size. The big issue with this behavior is that your UI is going to jump all around as images load, this makes for a very bad user experience.

In React Native, this behavior is intentionally not implemented. It is more work for the developer to know the dimensions (or just aspect ratio) of the image in advance, but we believe that it leads to a better user experience.

## Background Image via Nesting

A common feature request from developers familiar with the web is `background-image`. To handle this use case, simply create a normal `<Image>` component and add whatever children to it you would like to layer on top of it.

```javascript
return (
  <Image source={...}>
    <Text>Inside</Text>
  </Image>
);
```

## Off-thread Decoding

Image decoding can take more than a frame-worth of time. This is one of the major source of frame drops on the web because decoding is done in the main thread. In React Native, image decoding is done in a different thread. In practice, you already need to handle the case when the image is not downloaded yet, so displaying the placeholder for a few more frames while it is decoding does not require any code change.

## Static Assets

In the course of a project, it is not uncommon to add and remove many images and accidentally end up shipping images you are no longer using in the app. In order to fight this, we need to find a way to know statically which images are being used in the app. To do that, we introduced a marker on require. The only allowed way to refer to an image in the bundle is to literally write `require('image!name-of-the-asset')` in the source.

```javascript
// GOOD
<Image source={require('image!my-icon')} />

// BAD
var icon = this.props.active ? 'my-icon-active' : 'my-icon-inactive';
<Image source={require('image!' + icon)} />

// GOOD
var icon = this.props.active ? require('image!my-icon-active') : require('image!my-icon-inactive');
<Image source={icon} />
```

When your entire codebase respects this convention, you're able to do interesting things like automatically packaging the assets that are being used in your app. Note that in the current form, nothing is enforced, but it will be in the future.

## Best Camera Roll Image

iOS saves multiple sizes for the same image in your Camera Roll, it is very important to pick the one that's as close as possible for performance reasons. You wouldn't want to use the full quality 3264x2448 image as source when displaying a 200x200 thumbnail. If there's an exact match, React Native will pick it, otherwise it's going to use the first one that's at least 50% bigger in order to avoid blur when resizing from a close size. All of this is done by default so you don't have to worry about writing the tedious (and error prone) code to do it yourself.

## Source being an object

In React Native, one interesting decision is that the `src` attribute is named `source` and doesn't take a string but an object with an `uri` attribute.

```javascript
<Image source={{uri: 'something.jpg'}} />
```

On the infrastructure side, the reason is that it allows to attach metadata to this object. For example if you are using `require('image!icon')`, then we add an `isStatic` attribute to flag it as a local file (don't rely on this fact, it's likely to change in the future!). This is also future proofing, for example we may want to support sprites at some point, instead of outputting `{uri: ...}`, we can output `{uri: ..., crop: {left: 10, top: 50, width: 20, height: 40}}` and transparently support spriting on all the existing call sites.

On the user side, this lets you annotate the object with useful attributes such as the dimension of the image in order to compute the size it's going to be displayed in. Feel free to use it as your data structure to store more information about your image.

一个 react 的组件用以显示不同类型的图片，包括网络图片，静态资源，临时的本地图片，还有本地磁盘的图片，比如手机照片。

使用例子：

```` javascript
renderImages: function() {
  return (
    <View>
      <Image
        style={styles.icon}
        source={require('image!myIcon')}
      />
      <Image
        style={styles.logo}
        source={{uri: 'http://facebook.github.io/react/img/logo_og.png'}}
      />
    </View>
  );
},
````

## 支撑部分

**accessibilityLabel** string 

accessibilityLabel - 用来自定义更方便使用的字符串.

**accessible** bool 

accessible - 标识是否是一个可使用的元素.

**capInsets** {top: number, left: number, bottom: number, right: number} 

capInsets - 当图片的尺寸被改变时, capInsets 定义的角度大小会保持不变,但是图片的中心内容还有边框会被拉伸。但你想创建可以改变大小的圆角按钮，阴影还有其他可伸缩的元素时，这个属性会很重要。

[https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIImage_Class/index.html#//apple_ref/occ/instm/UIImage/resizableImageWithCapInsets:](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIImage_Class/index.html#//apple_ref/occ/instm/UIImage/resizableImageWithCapInsets:)

**source** {uri: string} 

**style** style 

├─[**Flexbox...**](http://facebook.github.io/react-native/docs/flexbox.html#proptypes)
├─**backgroundColor** string
├─**borderColor** string
├─**borderRadius** number
├─**borderWidth** number
├─**opacity** number
├─**resizeMode** Object.keys(ImageResizeMode)
├─**tintColor** string
└─**testID** string 

testID - 当运行 UI 自动化测试脚本的时候，这会作为一个唯一的标识符。

将图片显示出来是一件让人着迷的事情， 在这方面，React Native 为了让图片体验更好而浑身解数。