# PearlGoldOliveCoveredGlassDish 

## Screenshots

![screenshot Babylon.js](screenshot/screenshot_Large.jpg)

(above) [Babylon.js Sandbox](https://sandbox.babylonjs.com/) screenshot.

![screenshot](screenshot/glTFSampleViewer.jpg)

(above) [glTF Sample Viewer](https://github.khronos.org/glTF-Sample-Viewer-Release/) screenshot, with the [Babylon.js Studio HDRI](https://github.com/BabylonJS/Assets/blob/master/environments/studioSoftbox2Umbrellas_panorama_radiance.hdr).

## Description

This model represents a real Wayfair product, an [Olive 21 fl oz. Covered Dish](https://www.wayfair.com/kitchen-tabletop/pdp/bloomsbury-market-olive-21-fl-oz-covered-dish-w001802862.html?piid=1661795386), which has a glass cover with an iridescent glaze, and a glass dish with the same glaze and gold leaf applied to the underside. 

The use of real-world photographic reference is meant to help glTF developers with calibrating material features and renderer behavior, to more accurately represent e-commerce products. Additional photo reference is available [on the Wayfair website](https://www.wayfair.com/kitchen-tabletop/pdp/bloomsbury-market-olive-21-fl-oz-covered-dish-w001802862.html?piid=1661795386).

![screenshot Babylon.js](screenshot/ReferencePhotos.jpg)

(above) Photos of the real product.

The asset uses four extensions:

1. [KHR_materials_ior](https://github.com/KhronosGroup/glTF/tree/master/extensions/2.0/Khronos/KHR_materials_ior)
1. [KHR_materials_specular](https://github.com/KhronosGroup/glTF/tree/master/extensions/2.0/Khronos/KHR_materials_specular)
1. [KHR_materials_transmission](https://github.com/KhronosGroup/glTF/tree/master/extensions/2.0/Khronos/KHR_materials_transmission)
1. [KHR_materials_volume](https://github.com/KhronosGroup/glTF/tree/master/extensions/2.0/Khronos/KHR_materials_volume)

## Animation

The glassCover has an animation to rotate upward and back, revealing the olives inside, showcasing the refraction and specular effects on the glass.

![screenshot](screenshot/glassCover_animation.gif)

(above) Animation of the glassCover, as an animated GIF.

There are four materials: 

1. glassCover
1. glassDish
1. goldLeaf
1. olives

## glassCover Material

The glassCover material uses `KHR_materials_specular` to reproduce the colored specularity of the glaze, and `KHR_materials_ior` and `KHR_materials_transmission` and `KHR_materials_volume` for refraction. 

The specular colors are caused by a type of glaze called [carnival glass](http://www.ddoty.com/newcomers.html). On the real product the glaze is applied to the inside of the glass cover rather than the outside. However because most rasterizers render only a single layer of transmission, the specular colors were applied to both inside and outside of the cover rather than only on the inside.

Refraction is simulated using `KHR_materials_transmission` along with `KHR_materials_ior` and `KHR_materials_volume` and a `thicknessTexture` that guides rasterizers to render the solid-glass tapered top with a deeper refraction. This texture is ignored by pathtracers which use the actual thickness of the geometry rather than the thickness texture. A noise pattern has been added to the thickness to reproduce the wavy refractions seen in the photos.

![screenshot](screenshot/glassCover_textures.jpg)

(above) Textures for the glassCover material: base color, specular, thickness, normal.

## glassDish and goldLeaf Materials

The top of the glass dish uses a glass material with `KHR_materials_specular` for the colored glaze, and the bottom of the dish has gold leaf applied. Both surfaces use explicit geometry for front and back faces because the back side of each surface is visible behind the other material: from above the gold leaf is visible under the glass, and from below the glass can be seen through cracks in the goldleaf.

The glass uses only `KHR_materials_transmission` without `KHR_materials_ior` or `KHR_materials_volume` because it was preferred to have less distortion of the gold leaf visible underneath the glass. 

The gold leaf uses `"alphaMode":"MASK"` for the cracks instead of `KHR_materials_transmission` because most rasterizers render only a single layer of transmission. If transmission was used, the gold leaf would be invisible from above under the glass. 

![screenshot](screenshot/glassDish_goldleaf.jpg)

(above) The gold leaf surface, as seen from underneath the model.

## Olives

Olives have been added to assist in judging the refraction and occlusion details. 

The upper surfaces of the glassDish and goldLeaf use a precomputed ambient occlusion texture which simulates how the olives occlude lighting on the surfaces beliow them. Rasterizers may use this texture to occlude the IBL, while pathtracers will calculate the occlusion themselves.

![screenshot](screenshot/glassDish_olives.jpg)

(above) The ambient occlusion on the glass dish and gold leaf under the olives. 

![screenshot](screenshot/glassDish_olives_refraction.jpg)

(above) As the glass cover animates, the olives help to illustrate distortions caused by refraction through the glass. 

## Creation Details

The asset was created using [3ds Max](https://www.autodesk.com/products/3ds-max), exported to glTF using [Max2Babylon](https://github.com/BabylonJS/Exporters#babylonjs-exporters), and materials were edited in [Visual Studio Code](https://code.visualstudio.com/) with [glTF Tools](https://github.com/AnalyticalGraphicsInc/gltf-vscode#gltf-tools-extension-for-visual-studio-code).


## License Information

Copyright 2021 Wayfair LLC. CC BY 4.0 International https://creativecommons.org/licenses/by/4.0/
