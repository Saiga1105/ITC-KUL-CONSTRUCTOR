---
layout: post
title: Alignment Accuracy Competition
subtitle: 
cover-img: /assets/img/photogr2.PNG
thumbnail-img: /assets/img/29.PNG
share-img: 
tags:
---

## [COMPETITION](https://paperswithcode.com/datasets)

Did you know that only 65% of the imagery of the Construction sites could be alligned using commercial software ( i.e. [MetaShape](https://www.agisoft.com/) and [RealityCapture](https://www.capturingreality.com/))? This is not so surprising if you know the many obstacles [**Structure-from-Motion**](https://en.wikipedia.org/wiki/Structure_from_motion) pipelines have to overcome to allign the imagery below.

![37.PNG](../assets/img/37.PNG)

In this competition, we want to spark innovation for more robust alignment procedures. This includes the calculation of both the interior and exterior camera parameters. As an example, you can take a look at what information is stored from a software like [RealityCapture](https://www.capturingreality.com/)

![38.PNG](../assets/img/38.PNG)

Given these parameters, we can accurately position the cameras in the construction coordinate system. Using the [GEOMAPI](https://https://geomatics.pages.gitlab.kuleuven.be/research-projects/geomapi/) API, we can easily import some imagery and point clouds. For instance, in the example below, we load a set of images from a MetaShape xml file.

```
import geomapi
from geomapi import tools as tl
from geomapi.utils import geometryutils as gmu

imgNodes=tl.xml_to_nodes(path_to_metashape_xml)

las= laspy.read(las_path)
pcdNode=PointCloudNode(name='myPointCloud', resource=gmu.las_to_pcd(las))
```

And we can visualize these resources using [Open3D](http://www.open3d.org/) and some placeholder geometries for the imagery. Note that merging geometries before sending them to the visualizer significantly speeds up the rendering.

```
joinedImages=gmu.join_geometries([gmu.generate_visual_cone_from_image(n.cartesianTransform, height =1).paint_uniform_color([1,0,0]) for n in imgNodes])
o3d.visualization.draw_geometries([joinedImages]+[pcdNode.resource])
```

Now the [COMPETITION](https://paperswithcode.com/datasets) is to align as many of the resources as possible. All you have to do is report your percentage of properly aligned imagery on the complete dataset.

![2.PNG](../assets/img/2.PNG)


