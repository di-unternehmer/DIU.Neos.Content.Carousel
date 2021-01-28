
# DIU Neos Content Carousel

This package adds slider functionality to your site. We use Swiper for this package.
                                                     
https://swiperjs.com/


![Slider screenshot](Resources/Public/Images/screenshot.jpg)

Image Source: https://www.pexels.com/de-de/foto/arbeit-arbeitsplatz-buro-business-374720/


## Constraints in your site package (example)

To restrict the content elements which should be slideable you should overwrite the constraints in you site package like this example.

```
'DIU.Neos.Content.Carousel:Carousel':
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
prototype(DIU.Neos.Content.Carousel:Carousel) {
    renderer = DIU.Neos.Content.Carousel:Carousel.Presentation {
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
        templatePath = 'resource://My.Package/Private/Fusion/Carousel/Carousel.js'
    }
}
```
... containing:
```
var swiper = new Swiper('.swiper-container', {
  autoHeight: true, //enable auto height
  pagination: {
    el: '.swiper-pagination',
  },
});
```


## Extend the Carousel with properties

To add background colors just extend the properties in you site package.

```
'DIU.Neos.Content.Carousel:Cover':
  properties:
    backgroundColor:
      ui:
        inspector:
          editorOptions:
            values:
              'white':
                label: 'White'
    backgroundColorLeft:
      ui:
        inspector:
          editorOptions:
            values:
              'white':
                label: 'White'
    backgroundColorRight:
      ui:
        inspector:
          editorOptions:
            values:
              'white':
                label: 'White'

```

## If you want to use text color, first add these properties to your site package.

```
'DIU.Neos.Content.Carousel:Cover':
  properties:
    textColor:
      type: string
      defaultValue: 'text-color--default'
      ui:
        label: 'Text Color'
        reloadIfChanged: TRUE
        inspector:
          group: 'general'
          editor: 'Neos.Neos/Inspector/Editors/SelectBoxEditor'
          editorOptions:
            values:
              'text-color--default':
                label: Default
              'text-color--white':
                label: White
              'text-color--black':
                label: Black
    textColorLeft:
        ...
    textColorRight:
        ...
```

And connect them to the classlist with fusion in your site package.

```
prototype(DIU.Neos.Content.Carousel:Cover.Presentation) {
    renderer {
        class {
            textColor = ${q(node).property('textColor')}
        }
        lefttClass {
            textColorLeft = ${q(node).property('textColorLeft')}
        }
        rightClass {
            textColorRight = ${q(node).property('textColorRight')}
        }
    }
}
```
Don't forget to add custom css to your site package for these new classes which will be added now to the container.

```
.cover-container__wrapper {
  &.text-color--white,
  .text-color--white {
      p, a, li, h1, h2, h3, h4, h5, h6 {
          color: white;
      }
  }
  &.text-color--black,
  .text-color--black {
      p, a, li, h1, h2, h3, h4, h5, h6 {
          color: black;
      }
  }
  &.background-class--red,
  .background-class--red {
      background-color: red;
  }
}
```


## Prevent overwriting by dependency

To prevent overwriting the changes you made in your site package you have to require the carousel package in your site package composer.json. 

```
{
    "description": "Another Neos Site",
    "type": "neos-site",
    "require": {
        "neos/neos": "*",
        "diu/content-carousel": "*"
    },
    ...
}
