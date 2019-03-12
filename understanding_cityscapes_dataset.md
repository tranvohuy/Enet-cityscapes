Datasets for cityscapes can be downloaded [here](https://www.cityscapes-dataset.com/downloads/). (We have to create an account). There are two main files: leftImg8bit_trainvaltest.zip and gtFine_trainvaltest.zip. 

# leftImg8bit_trainvaltest.zip
True images are stored in leftImg8bit_trainvaltest.zip. In this folder, we can see real (reality/true/ground truth) pictures of roads, car driving, etc.
- The folder leftImg8bit_trainvaltest.zip when unzipped, will have sub-folders with names {aachen, frankfurt, etc}. Here is an example of the file 'aachen_000000_000019_leftImg8bit.png'
  
 <img src="https://raw.githubusercontent.com/tranvohuy/Enet-cityscapes/master/readme_files/aachen_000000_000019_leftImg8bit.png" width="70%">
  
- Each picture has its 'segmentation' verions which are stored in gtFine_trainvaltest.zip: _gtFine_color.png, _gtFine_instanceIds.png, _gtFine_labelIds.png, _gtFine_polygons.json

<img src="https://raw.githubusercontent.com/tranvohuy/Enet-cityscapes/master/readme_files/aachen_000000_000019_gtFine_color.png" width="40%">   <img src='https://raw.githubusercontent.com/tranvohuy/Enet-cityscapes/master/readme_files/aachen_000000_000019_gtFine_instanceIds.png' width ='40%'>
  
  !<img src='https://raw.githubusercontent.com/tranvohuy/Enet-cityscapes/master/readme_files/aachen_000000_000019_gtFine_labelIds.png' width = '40%'>
  
  
 - The true image is segmented (partitioned) into regions. Each region belongs to [34](https://github.com/mcordts/cityscapesScripts/issues/8) [classes](https://www.cityscapes-dataset.com/dataset-overview/#labeling-policy), [says](https://github.com/mcordts/cityscapesScripts/blob/master/cityscapesscripts/helpers/labels.py), car, [truck](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/helpers/labels.py#L91), [tree](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/helpers/labels.py#L85), [pole](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/helpers/labels.py#L81), [person](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/helpers/labels.py#L88), etc. Files in gtFine give 4 different ways to encode this partion.
 
# gtFine_trainvaltest.zip

- gtFine_color.png: <img src="https://raw.githubusercontent.com/tranvohuy/Enet-cityscapes/master/readme_files/aachen_000000_000019_gtFine_color.png" width="10%"> . This file is of type RGB. Each pixel is colored according to its classes. There is no distinstion between car1 or car2, etc. (RGB for [car](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/helpers/labels.py#L90) is (  0,  0, 142)) 

- gtFine_instanceIds.png: <img src='https://raw.githubusercontent.com/tranvohuy/Enet-cityscapes/master/readme_files/aachen_000000_000019_gtFine_instanceIds.png' width ='20%'>


- gtFine_labelIds.png:  <img src='https://raw.githubusercontent.com/tranvohuy/Enet-cityscapes/master/readme_files/aachen_000000_000019_gtFine_labelIds.png' width = '20%'>a grey-scale image. Each pixel has value (multiplied by 255) in [0,1,...,33,255]. Meaning of these numbers can be seen [here](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/helpers/labels.py#L63)
  


- gtFine_polygons.json: a json-type file. It stores information for each region (partition/ segmentation) of the true image. Each region has ```label``` and ```polygon```.  An example in aachen_000000_000019_gtFine_polygons.json
  ```
  {   "label": "pole", 
            "polygon": [
                [ 102, 278], 
                [ 103, 243], 
                [ 112, 243], 
                [ 111, 281]
            ]
    }, ```
      
This means, there is a 'pole' object. It is a polygon from pixels with coordinate (102,278)-(103,243)-(112,243)-(111,281)
      
The json files seem to have the most information about segmentation. We can see from the files that there are many object poles, 11 object cars, 2 object people, etc. One true car can be splitted into several object cars because of poles or trees in front of them.
 
 # Convert file types
 
 There is a way to convert json file to three other image file types using [json to instanceids](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/preparation/json2instanceImg.py), [json to labelids](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/preparation/json2labelImg.py)
