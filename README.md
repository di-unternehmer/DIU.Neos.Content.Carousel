
# DIU Neos Content Carousel

This package adds slider functionality to your site. We use Swiper for this package.
                                                     
https://swiperjs.com/


![Slider screenshot](Resources/Public/Images/screenshot.jpg)

Image Source: https://www.pexels.com/de-de/foto/arbeit-arbeitsplatz-buro-business-374720/


## Constraints in your site package (example)

To restrict the content elements which should be slideable you should overwrite the constraints in you site package like this example.

```
'DIU.Neos.Content.Carousel:Slide':
  constraints:
    nodeTypes:
      'Neos.Neos:Content': false
      'My.Package:Picture': true

```

## Required resources

This package autoincludes the required js and css into Neos.Neos:Page but you can prevent them to load und include it into your frontend build process by overwriting the fusion code in your site package.

```
prototype(Neos.Neos:Page) {
    sliderStyles = null
    sliderScript = null
}
```

You will need to include these files into your frontend build:

```
DIU.Neos.Content.Carousel/Resources/Public/JavaScript/swiper-bundle.min.js
DIU.Neos.Content.Carousel/Resources/Public/Styles/swiper-bundle.min.css
DIU.Neos.Content.Carousel/Resources/Private/Fusion/Script/Script.js
DIU.Neos.Content.Carousel/Resources/Private/Fusion/Styles/Styles.css 
```

Change the color of the navigation arrows in your site package:

```
.swiper-button-prev:after,
.swiper-container-rtl .swiper-button-next:after,
.swiper-button-next:after,
.swiper-container-rtl .swiper-button-prev:after {
  color: #ff620b;
}
```
## Customize swiper navigation and options

If you would like to use a different initialization you could overwrite the script.
For example, you just want to have the dotted navigation:

```
prototype(DIU.Neos.Content.Carousel:Wrapper) {
    renderer = DIU.Neos.Content.Carousel:Wrapper.Presentation {
        navigation = Neos.Fusion:Tag {
            tagName = 'div'
            attributes.class = 'swiper-pagination'
        }
    }
}
```

... add your own script:
```
prototype(DIU.Neos.Content.Carousel:Script) {
    initialize {
        templatePath = 'resource://My.Package/Private/Fusion/Slider/Slider.js'
    }
}
```
... containing:
```
var swiper = new Swiper('.swiper-container', {
  pagination: {
    el: '.swiper-pagination',
  },
});
```


